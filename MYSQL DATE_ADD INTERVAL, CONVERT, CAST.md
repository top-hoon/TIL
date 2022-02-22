## 시간 더하기, 빼기
 - DATE_ADD(기준 날짜, INTERVAL)
 - DATE_SUB(기준 날짜, INTERVAL)
 - EX) 
````SQL 
-- 1초 더하기
SELECT DATE_ADD(NOW(), INTERVAL 1 SECOND);
-- 1분 더하기
SELECT DATE_ADD(NOW(), INTERVAL 1 MINUTE);
-- 활용
DATE_FORMAT(DATE_ADD(sysdate(), INTERVAL cast({BASE_DAY} as unsigned) day), '%Y%m%d')
````


## CONVERT
 - 개인적으로 가장 많이 사용하는 데이터 변환 함수라고 생각합니다. 날짜 변환하는데도 유용하게 사용 가능합니다. 참고로 부동 소수점 또는 숫자에서 정수로 변환할 때 CONVERT() 함수는 결과를    자르고 다른 변환일 경우에는 반올림합니다.
 - 문법
````SQL
CONVERT(data_type[(length)], expression[style])
````
 - 예시
````SQL
SELECT CONVERT(NVARCHAR(10),칼럼) AS 칼럼명 FROM MY_TABLE --VARCHAR로 변환
SELECT CONVERT(INT,칼럼) AS 칼럼명 FROM MY_TABLE --INT로 변환
SELECT CONVERT(CHAR,칼럼) AS 칼럼명 FROM MY_TABLE --CHAR로 변환
````
 - 이외 
 - expression : 유효한 식
 - data_type : 대상 데이터 형식 별칭 데이터 형식은 사용할 수 없습니다.
 - length : 대상 데이터 형식의 길이를 지정하는 선택적 정수입니다. 기본값은 30입니다.
 - style : Convert함수가 식을 변환하는 방법을 지정하는 정수 식입니다. style이 Null이면 Null 값이 반환됩니다.

 - 예제
````SQL
--테이블(MY_TABLE)의 나이(AGE)칼럼을 INT에서 CHAR로 형변환--
SELECT CONVERT(NVARCHAR(10),AGE)+'세'AS 나이 FROM MY_TABLE

--테이블(MY_TALBE)에서 날짜(DTS)칼럼을 INT에서 DATE로 형변환--
SELECT CONVERT(DATE,SUBSTRING(DTS,1,8))AS 날짜 FROM MY_TABLE
````

## CAST
 - ※ FLOAT, 또는 NUMBERIC에서 INTEGER로 변환할 때 CAST() 함수는 결과를 자릅니다.
````SQL
--문법--
CAST(expression AS data_type(length))
--예시--
SELECT CAST(칼럼 AS INT) FROM  MY_TABLE
````
 - 예제
````SQL
--테이블(MY_TALBE)에서 가격(PRICE)칼럼을 INT에서 VARCHAR로 형변환
SELECT CAST(PRICEAS AS VARCHAR)AS 가격 FROM  MY_TABLE
````


