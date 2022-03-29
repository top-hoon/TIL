## 인덱스
### 사용
````sql
-- 인덱스 조회
show index from 테이블명
-- 인덱스 생성(둘 중 하나 사용)
create index 인덱스명 on 테이블명(컬럼명)
alter table 테이블명 add index 인덱스명(컬럼명)
-- 인덱스 삭제
alter table 테이블명 drop index 인덱스명;
-- 인덱스 타는지 확인하는법
EXPLAIN (type이 all, possible Key 가 null 일 경우에는 안탐  type이 ref일때 possible_key를 보쟈)
select * from tb_1
````
 - 인덱스는 무조건 타는게아님 db가 판단함
