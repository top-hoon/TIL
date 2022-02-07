## TIMESTAMPDIFF
  - 처음봤다 나는 쓸 일이 없었던것같다 아니 존재도 몰랐으니 안썼던것같다
  - 두 날짜간의 차이를 가져올 때 사용하는 mysql 함수이다 
  - 단순히 일 차이를 가져올 때 사용하는것이 DATEDIFF 이고 원하는대로 format을 지정해서 가져오는 함수는 TIMESTAMPDIFF 이다
  
### TIMEDIFF
  - DATEDIFF(날짜1, 날짜2); 날짜1- 날짜2 이다.
  - SELECT DATEDIFF('2018-03-28 23:59:59', '2017-03-01 00:00:00');
### TIMESTAMPDIFF
  - TIMESTAMPDIFF(단위, 날짜1, 날짜2); 날짜2 - 날짜1이다
  - 단위자리에는 
    - SECOND, MINUTE, HOUR, DAY, WEEK, MONTH, QUARTER(분기), YEAR 등이있다.
  - SELECT TIMESTAMPDIFF(MONTH, '2022-02-07', '2022-01-07');
  
## STR_TO_DATE()
  - DATE_FORMATE()의 반대. 문자열을 날짜형태로 바꿔준다 format에 주의하자
  - SELECT STR_TO_DATE('May 1, 2013', '%M %d, %Y');
  - SELECT STR_TO_DATE('a09:30:17', 'a%h:%i:%s');
  - format에 주의해서 쓰지 않으면 안뜨니 주의하길..
  
## INTERVAL
   DATE_FORMAT(DATE_ADD(STR_TO_DATE(A.ITEM_SDATE,'%Y%m%d'), INTERVAL - 1 MONTH),'%Y%m%d')
   
  //참고
  https://webobj.tistory.com/141
