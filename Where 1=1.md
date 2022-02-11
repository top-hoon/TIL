## WHERE 1=1 
  - 회사에서 코드 분석중에 이런게 나왔는데 where 절에는 항상 조건을 달았던 것 같은데 이게 뭐지 싶었다 바로 부장님한테 여쭤보려다 말고 구글링..
  - 일단 1=1 Boolean 처럼 참으로 나타나게 하는것이다 
  - 즉 그냥 다 뽑겠다는 얘기!!
   
### 장점
  - 쿼리 디버깅 시, 주석처리가 편하다고한다!
````sql 
SELECT * from Member  where mem_id = '1'
AND mem_name LIKE 'lee%'
````
    -  만약 이런 상황일 때 AND 이하 절이 잘 나오는지 확인하고 싶을 때, 
````sql 
SELECT * from Member  where 1=1
--mem_id = '1'
AND mem_name LIKE 'lee%'
````
    -  이런식으로 처리하면 비교적 쉽게 주석 처리하면서 디버깅이 가능하다!

  -  동적쿼리에서 특정 상황마다 WHERE 절을 다르게 작성해줘야 할 때 편리하다.
````java
query1 = "SELECT * FROM Member "

if(!memId.equals("") {
	query2 = "WHERE mem_id = '" + memId + "'"
}
if(!memName.equals("") {
	if(!memId.equals("") {
    	query3 = "AND"
    } else {
    	query3 = "WHERE"
    }
	query4 = "mem_name = '" + memName + "'"
}

````
    - 보기만 해도 귀찮다. 지금은 그나마 조건을 두개를 주는 상황이라서 괜찮지만, 나중에 3개 5개 이런식이면 좀 곤란하다 귀찮다.
    - 하지만!
````java
query1 = "SELECT * FROM Member WHERE 1=1 "

if(!memId.equals("") {
	query2 = "AND mem_id = '" + memId + "'"
}
if(!companyName.equals("") {
	query2 = "AND mem_name = '" + memName + "'"
}
````
- 이런식으로 하면 조금 편리하고 보기 좋게 만들수 있다!
### 뭐좀 검색해보다가 우연히 본 where 1=1 의 위험성
 - 만약 select문이 아닌, update문이나 delete문에 사용한다면
````java
<update id="test">
	update test_tbl
	set use_yn = 'N'
	where 1=1
	<if test="first != null">
		and first = #{first}
	</if>
</update>
````
 - first 값이 null이라면 WHERE 1=1 구문이 되면서 test_tbl 테이블의 모든 use_yn 컬럼 값은 'N'이 될 것이다. 따라서 중요한 것은 WHERE 1=1을 쓰지 말라는 것보다도 '예외 처리'를 확실히 하    는 것이 좋겠다 라고 했다 글쓴이도 그냥 그래도 뭐좀 알고 쓰라고 한것같다.
 - 검색을 위한 쿼리에서는 사용하면 효율적이지만 수정과 삭제를 하는경우에는 사용을 지양하는 것이 좋습니다.
   수정과 삭제시 WHERE 1=1를 사용하게되면 조건이 없을 경우 테이블의 모든 데이터가 변경 또는 소실될 우려가 있기 때문입니다. 
https://jdm.kr/blog/7


