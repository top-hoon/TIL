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
