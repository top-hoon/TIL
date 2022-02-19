## DUMP
 - 자신이 사용하고 있는 오라클 DB의 데이터를 다른 곳으로 옮기고 싶을때 어떻게 익스포트 임포트하눈지 알아보자!
 - DB를 덤프하고 임포트하는 수준에는 크게 3가지 수준이 있습니다. 
 - 첫째가 전체 단위, 둘째가 사용자 단위, 셋째가 테이블 단위입니다.    


1. 전체 단위(오라클 DB 전체)
덤프(익스포트) |
C:\) exp userid=system/비밀번호 file='C:\파일명.dmp' full=y
임포트 |
C:\) imp userid=system/비밀번호 file='C:\파일명.dmp' full=y


2. 사용자 단위(특정 계정/사용자의 DB )
덤프(익스포트) |
C:\) exp userid=계정명/비밀번호 file='C:\파일명.dmp' 
임포트 |
C:\) imp userid=계정명/비밀번호 file='C:\파일명.dmp' 


3. 테이블 단위(특정 계정의 특정 테이블만을 추출)
덤프(익스포트) |
C:\) exp userid=계정명/비밀번호 file='C:\파일명.dmp' tables=테이블명
임포트 |
C:\) imp userid=계정명/비밀번호 file='C:\파일명.dmp' 


출처: https://tenlie10.tistory.com/35 [게임 엔진/클라이언트 개발자]





