
[[관계 DB]]를 위한 표준 질의어

## 기능에 따른 분류
---
+ [[데이터 정의어(DDL)]]: CREATE, ALTER, DROP 등
+ [[데이터 조작어(DML)]]: SELECT, INSERT, DELETE, UPDATE 등. 여기서 SELECT은 특별히 질의어(Query)라고 한다
+ [[데이터 제어어(DCL)]]: GRANT, REVOKE 등

## 실습환경
---
1) 클라이언트가 서버에 서비스 요청
2) 서버([[Apache]])는 해당 정보를 주기 위해 [[PHP]]에게 스크립트 실행 요청
3) [[PHP]]는 미리 작성된 프로그램을 통해 [[MySQL]]에 요청
4) [[MySQL]]는 요청에 대한 결과를 [[PHP]]에 전달
5) [[PHP]]는 받은 결과를 [[HTML]]로 변경 후 서버에 전달
6) 서버([[Apache]])는 받은 [[HTML]] 파일을 클라이언트에 전달