## GROUP_CONCAT
  - 필요에 의해 서로 다른 결과를 한줄로 합쳐서 보여줘야 할 경우가 있다. 전체 결과값을 가져와서 for문을 돌며 문자열을 붙여도 되긴 하지만 
    SELECT 쿼리를 던질때 결과값으로 합쳐져 있는 문자열을 받는게 더 편하다. 싱기방기
    
  - select * from table
  -> 결과값 {'fruit':'수박','fruit':'사과','fruit':'바나나'}
  - select type, group_concat(name) from table group by type
  -> {'fruit':'수박', '사과' , '바나나'} 
  
  - group_concat을 기본적인 형태로 사용했을경우 문자열 사이에 쉼표(,)가 붙게 된다.
    구분자를 변경하고 싶을때는 아래와 같이 SEPARATOR '구분자' 를 붙여 준다.
  - select type, group_concat(name separator '|') from test group by type ;

  - 합쳐지는 문자열에 중복되는 문자열을 제거 할때는 distinct 를 사용한다.
  - select type, group_concat(distinct name) from test group by type ;

  - 문자열을 정렬하여 나타내고 싶으면 order by 를 이용한다.
  - select type, group_concat(distinct name order by name) from test group by type ;


### 정리
  - MySQL에서 group by 로 문자열을 합칠땐 group_concat 을 이용한다.

    1. 기본형 : group_concat(필드명)
    2. 구분자 변경 : group_concat(필드명 separator '구분자')
    3. 중복제거 : group_concat(distinct 필드명)
    4. 문자열 정렬 : group_concat(필드명 order by 필드명)

EX)
````SQL 
select COMCD as 센터코드, group_concat(ITEM_NM) as '프로그램명' from program_item
 where COMCD = 'JUNGNANG01'
   and PART_CD = '02'
   and PROGRAM_KIND = '01'
   and SPORTS_CD = '07'
   and ITEM_NM like '%성장기%';
````

## group by로 뽑아온 값중에 가장 큰 값 가져오기 -max() 
  - select MAX(CARD_NO) AS CARD_NOS, MEM_NO from mem_card where USE_YN = 'Y' group by MEM_NO
  - group by 를 mem_no로 잡고 회원 넘버가 가지고 있는 카드번호가 제일 큰값을 뽑아서 정렬할 수 있다.

## LPAD, RPAD 왼쪽 오른쪽에 특정문자를 원하는 자리수만큼 넣기
  - 왼쪽에 특정 문자를 원하는 자리수만큼 채워서 반환
  - 사용법 : LAPD(원본 문자열, 원하는 자리수, 채울문자열) SELECT LPAD('ABC',10,'0')  FROM DUAL;  결과 : 0000000ABC
  
  - 오른쪽에 특정 문자를 원하는 자리수만큼 채워서 반환
  - 사용법 : RPAD(원본문자열, 원하는 자리수, 채울 문자열) SELECT RPAD('ABC',10,'0') FROM DUAL; 결과 : ABC0000000

## NVL()
  - nvl 함수는 값이 Null인경우 지정값을 출력하고, null이 아니면 원래 값을 그대로 출력한다. 

## NVL2()
  - NVL2 함수는 Null이 아닌 경우 지정값1을 출력하고, Null인 경우 지정값2를 출력한다.

## Cast()
  - Cast 함수를 사용하면 지정한 값을 다른 데이터 타입으로 변환가능
  - cast(expr as type) 
  - 지정할 수 있는 데이터 타입
  - BINARY[(N)], CHAR[(N)] [charset_info], DATE, DATETIME, DECIMAL[(M[,D])], JSON, NCHAR[(N)], SIGNED [INTEGER], TIME, UNSIGNED [INTEGER]



   출처: https://fruitdev.tistory.com/16 [과일가게 개발자]
