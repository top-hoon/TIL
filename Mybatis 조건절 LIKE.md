## MyBatis / iBatis에서 조건절에 Like 검색시 처리하는 방법이다.

검색하고자 하는 필드명이 "title" 이고 해당 필드에서 검색할 내용을 파라미터를 "keyword" 라고 하면 아래와 같이 검색할 수 있다.

[MySQL]
title like CONCAT('%',#{keyword},'%')


[Oracle]
title like '%' ||  #{keyword} || '%'


[MSSQL]
title like '%' + #{keyword} + '%'



출처: https://fruitdev.tistory.com/60 [과일가게 개발자]
