## ANSI SQL 
 - DBMS(Oracle, My-SQL, DB2 등)에서 각기 다른 SQL을 사용하므로, 미국 표준 협회(American National Standards Institute)에서 이를 표준화하여 표준 SQL 문을 정립 시켜 놓은 것이다.
 - 특징 : 표준 SQL이기 때문에 DBMS의 종류에 제약을 받지 않는다. (MYSQL, ORACLE)즉 특정 벤더에게 종속적이지 않아서 다른 벤더의 DBMS로 교체하더라도 빠르게 다른
          벤더사를 이동할 수 있다. 특정DBMS의 이탈이 가속되는것도 ANSI SQL의 영향이 크다고 할 수 있다. 
 - 테이블간의 JOIN 관계가 FROM에서 명시되기 때문에 WHERE문에서 조건만 확인하면된다. 즉 가독성이 일반 QUERY문보다 좋다.

### SAMPLE
````SQL
create table table1
(
    num int,
    name varchar(10)
)

insert into Test1 values ( 1, '삼성');
insert into Test1 values ( 2, '엘지');
insert into Test1 values ( 3, '애플');
insert into Test1 values ( 4, '레노버');

create table table2
(
    num int,
    name varchar(20)
)

insert into Test2 values ( 1, '갤럭시 플립');
insert into Test2 values ( 2, '그램');
insert into Test2 values ( 3, '맥북 프로');
````

### LEFT OUTER JOIN
````SQL
-- ORACLE
SELECT table1.*, table2.name
FROM table1, table2
WHERE table1.num = table2.num(+)

-- ANSI SQL
SELECT table1.*, table2.name
FROM table1
LEFT OUTER JOIN table2
ON table1.num = table2.num
````

### RIGHT OUTER JOIN
````SQL
-- ORACLE
SELECT table1.*, table2.name
FROM table1, table2
WHERE table1.num(+) = table2.num

-- ANSI
SELECT table1.*, table2.name
FROM table1 RIGHT OUTER JOIN table2
ON table1.num = table2.num
````
### FULL OUTER JOIN
````SQL
-- ORACLE
SELECT table1.*, table2.name
FROM table1, table2
WHERE table1.num = table2.num(+)

-- ANSI
SELECT table1.*, table2.name
FROM table1
FULL OUTER JOIN table2
ON table1.num = table2.num
````
- https://dejavuhyo.github.io/posts/ansi-sql/#3-right-outer-join

- https://dejavuhyo.github.io/posts/ansi-sql/#3-right-outer-join
