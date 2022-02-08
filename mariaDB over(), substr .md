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




