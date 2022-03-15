## DATE_FORMAT
  - DATE_FORMAT(날짜 , 형식) : 날짜를 지정한 형식으로 출력
  - 구분기호
````sql 
SELECT DATE_FORMAT(NOW(),'%Y-%m-%d') AS DATE FROM DUAL
````
  -  결과 
  -  2016-09-22

## abs()
 - x 의 절대값을 구하는 함수
````sql

 select abs(1000);
             -> 1000
 select abs(-1000);
             -> 1000
  select abs(-1156);
             -> 1156
````
