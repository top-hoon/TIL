## 프로시져
 - 여러쿼리를 한번에 수행하는것
 - SQL의 함수와 유사하지만 차이가있다
 - 함수 : 클라이언트에서처리하고, 리턴값이 필수이고 리턴값 하나만 반환가능하다.
 - 프로시져 : 서버로 보내섳퍼리하고, 리턴값을 선택하고, 리턴값을 여러개 반환한다
 - SQL처리를 하는 위치에서의 차이점인데, 속도면에서는 프로시져가 더 빠르다고하다 
   그래서 각각의 용도는 프로시저 같은 경우 실행, 처리를 할 때 주로 사용되고, 함수는 간단한 계산이나 수치 결과를 나타낼 때 주로 사용한다
### 프로시저는 처리 성능과 재사용면에서 좋지않다. 하지만 그럼에도 프로시저를 사용해야하는이유
 - 하나의 요청으로 여러 SQL문을 실행 가능
 - 네트워크 소요 시간을 줄일 수 있음(여러개의 쿼리를 처리하는 시점에서 네트워크 부하 줄임)
 - 보수성이 뛰어나다.
 - 개발 업무를 구분하여 개발할 수 있다. (DB관련 처리를 API처럼 만들어 제공)

 - 문법과 예시
````SQL
DELIMITER $$
CREATE PROCEDURE 'TEST_PROC' (
    -- 파라미터 선언
    PARAM_NAME VARCHAR(20),
    PARAM_AGE INT
)
BEGIN
    -- 변수 선언
    DECLARE PARAM_NUM INTEGER;
    
    -- 쿼리문1
    SELECT COUNT(*) + 1
    	INTO PARAM_NUM
        FROM table1;
        
    -- 쿼리문2
    INSERT INTO table1(total_count, user_name, user_age) VALUES(PARAM_NUM, PARAM_NAME, PARAM_AGE);
END $$
DELIMITER ;
````
 - 파라미터 선언은 프로시저명() 안에서 선언하고 SQL문과 변수 선언은 BEGIN ~ END 사이에 작성한다.
 - 그리고 SELECT 사용 시에는 조회한 컬럼(데이터)을 반드시 INTO로 변수 안에 넣어줘야하며,
 - 프로시저 내부에서 사용하는 SQL문은 일반 SQL문이기 때문에 세미콜론(;)으로 문장을 끝맺어야한다.
 - 첫번째와 마지막 라인에 DELIMITER라는 이상한 단어가 있는걸 확인할 수 있는데 프로시저 작성이 완료되지 않았음에도 SQL문이 실행되는 것을 막기 위해 사용된다.
 - (구분자(;)를 다른 구분자로 바꿨다가 프로시저 작성이 끝나면 다시 구분자를 원래대로 되돌림)

 - 호출
````SQL
CALL TEST_PROC('테스트이름', 21);
````
 - 이처럼 프로시저 호출은 다음과 같이 CALL 프로시저명(파라미터); 로 사용해 함수처럼 손쉽게 사용할 수 있다
 - 프로시저를 호출하면 MySQL은 DataBase카탈로그에서 프로시저 이름을 찾아 SQL문을 컴파일하고 메모리 공간(Cache)에 저장한 뒤 프로시저를 실행시킨다.
 - 또한 If, case, loop 같은 제어,반복문을 사용해 보다 향상된 SQL코드를 작성할 수 있고 프로시저 내에서 다른 프로시저를 호출할 수도 있다.

#### DB카탈로그 -> SQL컴파일 -> 메모리저장 -> 실행

 - 예시 (IN, OUT, INOUT 사용)
````SQL
DELIMITER $$
CREATE PROCEDURE 'TEST_PROC2'(
    IN loopCount1 INT,     -- input : 10
    IN loopCount2 INT,     -- input : 20
    OUT rst1 INT,
    OUT rst2 INT,
    INOUT rst3 INT
)
BEGIN
    DECLARE NUM1 INTEGER DEFAULT 0;    -- DEFAULT : 초기값 설정
    DECLARE NUM2 INTEGER DEFAULT 0;
    DECLARE NUM3 INTEGER DEFAULT 0;
    
    WHILE NUM1<loopCount1 DO           -- NUM1은 0~9까지 10번반복
        WHILE NUM2<loopCount2 DO       -- NUM2는 0~19까지 20번반복
            SET NUM3 = NUM3 + 1;
            SET NUM2 = NUM2 + 1;
        END WHILE;                     -- NUM2가 19가 되면 나옴
        
        SET NUM1 = NUM1 + 1;
        SET NUM2 = 0;
    END WHILE;
    
    SET rst1 = NUM1;
    SET rst2 = NUM3;
    SET rst3 = rst1 + rst2 + rst3;
END $$
DELIMITER ;
````

#### IN
 - 프로시저에 값을 전달하며, 프로시저 내부에서 값을 수정할 수는 있지만 프로시저가 반환되고 나서 호출자가 수정은 불가능하다.
   즉 원본 값은 프로시저가 끝난 후에도 유지되며, 프로시저는 IN 파라미터의 복사본을 사용한다.
   
#### OUT
 - 프로시저의 값을 호출자에게 다시 Return한다. 초기값은 프로시저 내에서 NULL이며 프로시저가 반환될 때 새로운 값이 호출자에게 Return되고 프로그램이 시작될 때,
   OUT 파라미터의 초기값에 접근할 수 없다.
   
#### INOUT
 - 호출자에 의해 하나의 변수가 초기화되고 프로시저에 의해 수정된다. 간단하게 IN+OUT이라고 생각하면 될 것 같다. 프로시저가 Return될 때 프로시저가 변경한 사항은 호출자에게 Return된다.

#### 예시(호출)
````SQL
-- 변수 초기화
DECLARE @NUM1 = 0;
DECLARE @NUM2 = 0;
DECLARE @NUM3 = 0;

-- NUM3에 값 30 할당(@는 전역변수, 프로시저가 끝나도 계속 유지되는 값)
SET @NUM3 = 30;
-- (10(IN), 20(IN), Return받을 변수(OUT), Return받을 변수(OUT), Return도 받고 값도 가지고 있는 변수(INOUT))
CALL TEST_PROC2(10, 20, @NUM1, @NUM2, @NUM3);

SELECT @NUM1, @NUM2, @NUM3;
-- RESULT => @NUM1 : 10, @NUM2 : 200, @NUM3 : 240
````


### 추가 문법 및 용어정리
 - 프로시져 목록확인
````SQL
SHOW PROCEDURE STATUS;
````
 - 프로시저 내용 확인
````SQL
SHOW CREATE PROCEDURE 프로시저이름;
````
 - 프로시저 삭제
````SQL
DROP PROCEDURE 프로시저이름;
````
 - 프로시져 목록확인
````SQL
SHOW PROCEDURE STATUS;
````
 - 조건문(IF)예시
````SQL
IF total >= 95 AND total <= 100 THEN
    SET grade = 'A+';
ELSE
    IF total >= 90 THEN
        SET grade = 'A';
    END IF;
END IF;
````
 - 조건문(CASE)예시
````SQL
CASE NUM
    WHEN 'Apple' THEN
        SET 'result_Str' = 'apple_cookie';
    WHEN 'Banana' THEN
        SET 'result_Str' = 'banana_cookie';
    ELSE
        SET 'result_Str' = 'orange_cookie';
END CASE;
````
##### 회사 프로시져예제
````SQL
CREATE DEFINER=`jnimc`@`%` FUNCTION `jnimc`.`Fn_GetHoliday`(P_COMCD varchar(20), P_DATE varchar(8), P_CLASS_CD varchar(10) CHARSET utf8) RETURNS varchar(8) CHARSET utf8
BEGIN  
    DECLARE 
      RTN_DATE varchar(8);
 
      select nvl(max(CAL_DATE), 'X') into RTN_DATE
        from (	
			     select GUBUN as CAL_DATE  from CALENDAR
			      where COMCD = P_COMCD
			        and CAL_DATE = P_DATE
			        and GUBUN = 'A'
			     union
			     select GUBUN as CAL_DATE from CALENDAR
			      where COMCD = P_COMCD
			        and CD = P_CLASS_CD
			        and CAL_DATE = P_DATE
			        and GUBUN = 'C'
	          ) XX;

     RETURN RTN_DATE; 
end
````
 - https://spiderwebcoding.tistory.com/7
