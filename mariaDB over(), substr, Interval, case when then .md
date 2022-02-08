## over() 함수
  - OVER함수는 ORDER BY, GROUP BY 서브쿼리를 개선하기 위해 나온 함수라고 할 수 있습니다.
  - COUNT(*)OVER() : 전체행 카운트
    COUNT(*)OVER(PARTITION BY 컬럼) : 그룹단위로 나누어 카운트

  - MAX(컬럼)OVER() : 전체행 중에 최고값
    MAX(컬럼)OVER(PARTITION BY 컬럼) : 그룹내 최고값

  - MIN(컬럼)OVER() : 전체행 중에 최소값
    MIN(컬럼)OVER(PARTITION BY 컬럼) : 그룹내 최소값

  - SUM(컬럼)OVER() : 전체행 합
    SUM(컬럼)OVER(PARTITION BY 컬럼) : 그룹내 합

  - AVG(컬럼)OVER() : 전체행 평균
    AVG(컬럼)OVER(PARTITION BY 컬럼) : 그룹내 평균

  - STDDEV(컬럼)OVER() : 전체행 표준편차
    STDDEV(컬럼)OVER(PARTITION BY 컬럼) : 그룹내 표준편차

  - RATIO_TO_REPORT(컬럼)OVER() : 현재행값/SUM(전체행값) 퍼센테이지로 나타낼경우 100곱하면 됩니다.
    RATIO_TO_REPORT(컬럼)OVER(PARTITION BY 컬럼) : 현재행값 / SUM(그룹행값) 퍼센테이지로 나타낼경우 100곱하면 됩니다.
    
  - SELECT MENU_ID, MENU_NAME, COUNT(*)OVER() AS TOTALCOUNT FROM mem_salse
  - 쉽게 전체 카운트를 할수 있다.

## substr()
  - SUBSTR(A.SALE_DATE,1,6) A.SALE_DATE 를 1번째부터 6개의 문자열을 추출
    - ex) 20220208 -> 202202


## interval (DATE_ADD)
  - 현재시간에 1초 더하기
    - SELECT DATE_ADD(NOW(), INTERVAL 1 SECOND)
  - 현재시간에 1달 빼기
    - SELECT DATE_ADD(NOW(), INTERVAL -1 MONTH)

## CASE WHEN THEN 
  - CASE WHEN 조건1 THEN 조건만족시 값1
         WHEN 조건2 THEN 조건만족시 값2
         WHEN 조건3 THEN 조건만족시 값3
         ELSE 0
    END 결과 컬럼명
    
  - 조건과 조건만족시 결과값의 쌍이 반복되며 모든 조건이 불일치 할 때는 ELSE 조건에 지정된 값을 결과로 얻는다. END 다음에는 지정하는 결과 컬럼명이 쿼리 결과 출력시 표시되는 컬럼명.
  - SELECT 판매량, 국적, 판매제품, CASE WHEN 국적 = '한국' THEN '고품질' ELSE 'x' END 고품질여부 FROM PANDA;
    - ex) SUM(CASE WHEN 국적 = '한국'THEN 판매량 ELSE 0 END) 한국_판매량
      ->  국적이 한국이면 판매량을 뽑고 아니면 0을 반환하는데 한국_판매량으로 출력
    
