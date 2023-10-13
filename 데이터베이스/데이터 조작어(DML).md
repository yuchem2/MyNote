## Data Manipulation Language

[[데이터(Data)]]의 삽입, 삭제, 검색 등의 처리를 요구하기 위해 사용하는 명령어
+ [[절차적 데이터 조작어(procedural DML)]]
+ [[비절차적 데이터 조작어(nonprocedural DML)]]

[[SQL(Structured Query Language)]]에서 테이블에 데이터를 검색, 삽입, 수정, 삭제하는 명령어

## SQL문 예시
---
명령어 문법은 모두 [[MySQL]]의 문법에 기초하였다
1. 데이터 검색: SELECT
 ```MySQL
 SELECT [ALL | DISTINCT] attribute_list FROM table_list [WHERE conditions];
 SELECT 제품명, 단가 AS 가격 FROM 제품;
 SELECT 제품명, 단가 '가 격' FROM 제품;
 SELECT 제품명, 단가 + 500 AS '조정 단가' FROM 제품;
 SELECT 주무제품, 수량, 주문일자, 주문 고객 FROM 주문 WHERE 주문고객='apple' OR 수량 >= '15';
 ```
   + SELECT 키워드와 함께 검색하고 싶은 속성의 이름 나열
   + FROM 키워드와 함께 검색하고 싶은 속성이 있는 테이블의 이름을 나열
   + WHERE 키워드와 함께 검색 조건을 제시해 그 조건에 해당하는 투플만 나열 가능
   + ALL: 결과 테이블이 투플의 중복을 허용하도록 지정
   + DISTINCT: 결과 테이블이 투플의 중복을 허용하지 않도록 지정
   + AS 키워드를 이용해 결과 테이블에서 속성의 이름을 바꾸어 출력 가능 (새로운 이름에 공백이 포함된 경우 따음표를 통해 입력해야함), 생략 가능
   + 속성의 이름과 산술 연산자를 통해 계산된 결과값을 보여줄 수 있음(원본 값 변경x)

1. 데이터 삽입: INSERT
```MySQL
 INSERT INTO table_name VALUES (attribute_value_list);
 ```
3. 데이터 수정: UPDATE
	
4. 데이터 삭제: DELETE

