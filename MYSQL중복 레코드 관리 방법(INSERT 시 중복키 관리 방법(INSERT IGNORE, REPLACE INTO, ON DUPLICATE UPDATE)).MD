## 중복 레코드 관리
 1. INSERT IGNORE
  - 최초 입수된 레코드가 남아 있음
  - 최초 입수된 레코드의 AUTO_INCREMENT 값은 변하지 않음

 2. REPLACE INTO
  - 최초 입수된 레코드가 삭제되고, 신규 레코드가 INSERT됨
  - TO_INCREMENT의 값이 변경됨

 3. INSERT INTO ON DUPLICATE UPDATE
  - INSERT IGNORE의 장점 포함함
  - 중복 키 오류 발생 시, 사용자가 UPDATE될 값을 지정할 수 있음


### 사전조건
 - 중복 레코드 관리를 위해선 테이블에 PRIMARY KEY 혹은 UNIQUE INDEX가 필요하다.
````SQL
CREATE TABLE person
(
  id INT NOT NULL AUTO_INCREMENT,
  name VARCHAR(20),
  address VARCHAR(255),
  PRIMARY KEY (id),
  UNIQUE INDEX (name) -- 중복 검사용 필드
);
````
 - PERSON 이라는 테이블에서 PK로 지정된 ID 필드는 AUTO_INCREMENT를 위해 만든 필드이며, 여기서는 NAME을 이용하여 중복검사함!
 - 중간에추가하려면~
````SQL
-- PRIMARY 추가하는 방법
ALTER TABLE person ADD PRIMARY KEY (name)

-- UNIQUE INDEX를 추가하는 방법
ALTER TABLE person ADD UNIQUE INDEX (name)
````
### 중복처리방법!

#### INSERT IGNORE
 - INSERT IGNORE는 중복 키 에러가 발생했을 때 신규로 입력되는 레코드를 무시하는 단순한 방법이다.
````SQL
mysql> INSERT IGNORE INTO person VALUES (NULL, 'James', 'Seoul');
Query OK, 1 row affected (0.00 sec)

mysql> INSERT IGNORE INTO person VALUES (NULL, 'Cynthia', 'Yongin');
Query OK, 1 row affected (0.00 sec)

mysql> INSERT IGNORE INTO person VALUES (NULL, 'James', 'Seongnam');
Query OK, 0 rows affected (0.00 sec)

mysql> INSERT IGNORE INTO person VALUES (NULL, 'Cynthia', 'Seoul');
Query OK, 0 rows affected (0.00 sec)

mysql> INSERT IGNORE INTO person VALUES (NULL, 'James', 'Incheon');
Query OK, 0 rows affected (0.00 sec)
````
 - 다음의 예를 보면 중복 키 에러가 발생했을 때 INSERT 구문 자체는 오류가 발생하지 않고, 대신’0 row affected’가 출력된 것을 볼 수 있다.
 - SELECT 결과는 2개만뜬다! 1. James(Seoul), 2. Cynthia(Yongin)
 - 최초에 입력한 정보만 나옴!
 - ID 를 보면 1,2인데, 중복발생 여부에 따라 식별 키가 변경되는 경우 여러가지 불편한점이 나오지만 INSERT IGNORE 에서는 AUTO_INCREMENT의 값이 변경되지 않는다는 장점이 있다.

#### REPLACE INTO
 - REPLACE INTO는 중복이 발생되었을 때 기존 레코드를 삭제하고 신규 레코드를 INSERT하는 방식이다.
````SQL
mysql> REPLACE INTO person VALUES (NULL, 'James', 'Seoul');
Query OK, 1 row affected (0.00 sec)

mysql> REPLACE INTO person VALUES (NULL, 'Cynthia', 'Yongin');
Query OK, 1 row affected (0.00 sec)

mysql> REPLACE INTO person VALUES (NULL, 'James', 'Seongnam');
Query OK, 2 rows affected (0.00 sec)

mysql> REPLACE INTO person VALUES (NULL, 'Cynthia', 'Seoul');
Query OK, 2 rows affected (0.00 sec)

mysql> REPLACE INTO person VALUES (NULL, 'James', 'Incheon');
Query OK, 2 rows affected (0.00 sec)
````
 - 세번째 레코드를 입수할 때부터는 ‘2 rows affected’가 출력되었다. (INSERT IGNORE에서는 ‘0 rows affected’가 출력되었었다)
 - SELECT 결과 4. CYNTHIA SEOUL, 5. JAMES INCHEON
 - 중복 키 오류 발생 시 기존 레코드를 삭제 - > 첫 번째 레코드가 AFFECTED 되었음
 - 이후 새로운 레코드를 입력 - > 두번째 레코드가 AFFECTED 되었음
 - “REPLACE INTO”는 그다지 좋은 방법이 아닌데 앞서 이야기한 것처럼 AUTO_INCREMENT는 레코드를 식별할 수 있는 id로 사용되는데 중복 발생 시 id가 변경되기 때문이다.

#### ON DUPLICATE UPDATE
 - ON DUPLICATE UPDATE의 장점은 중복 키 오류 발생 시 사용자가 원하는 값을 직접 설정할 수 있다는 점이다.
````SQL
mysql> INSERT INTO person VALUES (NULL, 'James', 'Seoul')
           ON DUPLICATE KEY UPDATE address = VALUES(address);
Query OK, 1 row affected (0.00 sec)

mysql> INSERT INTO person VALUES (NULL, 'Cynthia', 'Yongin')
           ON DUPLICATE KEY UPDATE address = VALUES(address);
Query OK, 1 row affected (0.00 sec)

mysql> INSERT INTO person VALUES (NULL, 'James', 'Seongnam')
           ON DUPLICATE KEY UPDATE address = VALUES(address);
Query OK, 2 rows affected, 1 warning (0.01 sec)

mysql> INSERT INTO person VALUES (NULL, 'Cynthia', 'Seoul')
           ON DUPLICATE KEY UPDATE address = VALUES(address);
Query OK, 2 rows affected, 1 warning (0.00 sec)

mysql> INSERT INTO person VALUES (NULL, 'James', 'Incheon')
           ON DUPLICATE KEY UPDATE address = VALUES(address);
Query OK, 2 rows affected, 1 warning (0.01 sec)
````
 - SELECT 결과 1. JAMES(INCHEON), 2. CYNTHIA(SEOUL)
 - 이번에는 id 값이 최초 입수된 레코드의 값 그대로이다. 하지만 address의 값이 마지막에 입수한 레코드로 변경되어 있다.
 - INSERT INTO ... ON DUPLICATE KEY UPDATE의 장점은 중복 발생 시 필드의 값을 내 맘대로 UPDATE할 수 있다는 점이다
 - id 필드만 놓고 보면 INSERT IGNORE와 동일하지만 address의 값이 변경된 것이 INSERT IGNORE와 INSERT INTO ... ON DUPLICATE KEY UPDATE의 차이점이다.
 - ON DUPLICATE KEY UPDATE를 이용하면 재미있는 것들을 할 수 있다. 중복 레코드가 총 몇 번이나 입수되었는지를 기록해보자.

````SQL
CREATE TABLE person
(
  id INT NOT NULL AUTO_INCREMENT,
  name VARCHAR(20),
  address VARCHAR(255),
  inserted_cnt INT, -- 레코드가 몇 번 입수되었는지 확인용 필드
  PRIMARY KEY (id),
  UNIQUE INDEX (name)
);
mysql> INSERT INTO person VALUES (NULL, 'James', 'Seoul', 1)
           ON DUPLICATE KEY UPDATE inserted_cnt = inserted_cnt + 1;
Query OK, 1 row affected (0.00 sec)

mysql> INSERT INTO person VALUES (NULL, 'Cynthia', 'Yongin', 1)
           ON DUPLICATE KEY UPDATE inserted_cnt = inserted_cnt + 1;
Query OK, 1 row affected (0.00 sec)

mysql> INSERT INTO person VALUES (NULL, 'James', 'Seongnam', 1)
           ON DUPLICATE KEY UPDATE inserted_cnt = inserted_cnt + 1;
Query OK, 2 rows affected (0.00 sec)

mysql> INSERT INTO person VALUES (NULL, 'Cynthia', 'Seoul', 1)
           ON DUPLICATE KEY UPDATE inserted_cnt = inserted_cnt + 1;
Query OK, 2 rows affected (0.00 sec)

mysql> INSERT INTO person VALUES (NULL, 'James', 'Incheon', 1)
           ON DUPLICATE KEY UPDATE inserted_cnt = inserted_cnt + 1;
Query OK, 2 rows affected (0.00 sec)
````
 - SELECT 해보면 JAMES 3번 CYNTHIA 2번임
 - 주의해야 할 점은 새로온 레코드의 값으로 UPDATE하고자 할 때는 항상 “VALUES(column)”과 같이 VALUES()로 감싸야 한다는 점이다. 만약 다음과 같이 VALUES() 없이 필드명만 사용하는 것은      기존 레코드의 컬럼 값을 의미하게 된다.

````SQL
INSERT INTO person VALUES (NULL, 'James', 'Incheon')
    ON DUPLICATE KEY UPDATE address = address;
````
 - 요렇게하면 UPDATE가 안됨.


- http://jason-heo.github.io/mysql/2014/03/05/manage-dup-key2.html 감사함다

