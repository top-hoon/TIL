## ON DUPLICATE KEY UPDATE
  - 데이터 삽입시, PRIMARY KEY나 UNIQUE KEY가 중복 되었을 경우 지정한 데이터만  UPDATE하는 명령어를 의미한다.(중복된 키가 없을 경우 insert 로직을 수행!)
  - 한마디로 insert문과 update문을 같이써놓고 특정 컬럼이 중복되었을때 insert가 아닌 해당 로우에 데이터를 업데이트한다는것


## INSERT IGNORE
  - 중복키 제약조건에 위배되면 INSERT 무시
````sql
   INSERT IGNORE INTO tb_member (tb_name, tb_price, tb_cnt) VALUES ('top-hoon', 100000, 2);
````


## REPLACE INTO
  - 중복키 제약조건에 위배되면 해당 레코드를 삭제하고 다시 삽입한다.
````sql
    REPLACE INTO tb_member (tb_name, tb_price, tb_cnt) VALUES ('top-hoon', 100000, 2);
````

## ROWNUM
  - 오라클에서 지원하는 가상컬럼 쿼리의 결과에 1부터 하나씩 증가하여 붙는 컬럼!
````sql
  SELECT mem_id , ROWNUM FROM tb_mem
````
  - 사용용도 : 주로 여러개의 결과를 출력하는 쿼리문을 실행 후 결과의 개수를 제한하여 가져오는데 사용한다.
  - 유의점 : ORDER BY 와 함께 사용할 경우 rownum이 먼저 순번을 매긴 후 ORDER BY 절을 실행하기 때문에 순번대로 정렬하기 위해서는 ORDER BY 절을 서브 쿼리를 이용하여 정렬한 후에 ROWNUM을 사용한다.
ex)
````sql
SELECT 컬럼명, ROWNUM FROM(SELECT 컬럼명 FROM 테이블 ORDER BY 컬럼명)
````

````sql
FROM 절 에서 초기화 후 사용
SELECT @ROWNUM:=@ROWNUM+1 AS rowNum FROM 테이블명, (SELECT @ROWNUM:=0) AS R

WHERE 절 에서 초기화 후 사용
SELECT @ROWNUM:=@ROWNUM+1 AS rowNum FROM 테이블명 WHERE (@ROWNUM:=0)=0;

INNER JOIN에 초기화 후 사용
SELECT 컬럼명, @ROWNUM:=@ROWNUM+1 AS rowNum FROM 테이블명 INNER JOIN (SELECT @rownum:=0) R

개수 제한하기
LIMIT 사용
SELECT * FROM(SELECT 컬럼명 ,@ROWNUM:=@ROWNUM+1 as rowNum FROM 테이블명 ,(SELECT @ROWNUM:=0) AS R ) T
LIMIT 0,14
````
