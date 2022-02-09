## 셀프 조인 : 자기 자신에게 테이블을 조인하는 법
  - 셀프조인은 2개의 서로 다른 테이블 내에있는 데이터를 하나의 테이블로 붙이는 대신 자기 자신안에 있는 똑같은 데이터를 가져다 붙이는것
  - why? 

|customer_id|firstname|lastname|birthdate|spouse_id|
|-----------|----------|-------|---------|---------|
|1|John|Mayer|1983-05-12|2|
|2|Mary|Mayer|1983-05-12|1|
|3|Lisa|Ross|1983-05-12|5|
|4|Anna|Timothy|1983-05-12|6|
|5|Tim|Ross|1983-05-12|3|

  - 이런식으로 구성되어있을때 1번과 2번이 부부라할때, 배우자의 id 값은 알수 있지만, 배우자의 이름과 성을 바로 알 수가 없다 그럴 때, 셀프조인을 해서 똑같은 데이터를 붙여준다면 

````sql
SELECT
 cust.customer_id,
 cust.firstname,
 cust.lastname,
 cust.birthdate,
 cust.spouse_id,
 spouse.firstname AS spouse_firstname,
 spouse.lastname AS spouse_lastname
FROM customer AS cust
INNER JOIN customer AS spouse
   ON cust.spouse_id = spouse_id;
````
  - 바로 배우자의 이름과 성을 알 수 있다.

참조 https://kimsyoung.tistory.com/entry/SELF-JOIN-%E4%B8%8A-%EA%B0%99%EC%9D%80-%ED%85%8C%EC%9D%B4%EB%B8%94%EC%9D%84-%EC%A1%B0%EC%9D%B8%ED%95%98%EA%B8%B0
