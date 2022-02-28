### ROUND()
 - 지정하는 소숫점 자리로 반올림한다.
````SQL
SELECT ROUND(135.375, 2); -- 135.38
````

### FLOOR()
 - 가장 가까운 '정수'로 버림하여 반환한다.
````SQL
SELECT FLOOR(135.375); -- 135
````

### CEIL ()
 - 가장 가까운 '정수'로 올림하여 반환한다
````SQL
SELECT CEIL(135.375); -- 136
SELECT CEILING(135.375); -- 136
````

### TRUNCATE()
 - 입력한 N번째 숫자까지 남기고 제거
````SQL
SELECT TRUNCATE(135.375, 2); --135.37
````

### count(*), count(1)
 - NULL값과 상관없이 모든 행 수를 카운트 한다.
 - count(컬럼) 은 해당 컬럼의 값의 NULL인 행은 COUNT에 포함되지 않는다.
