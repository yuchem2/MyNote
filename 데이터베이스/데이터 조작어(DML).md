## Data Manipulation Language

[[데이터(Data)]]의 삽입, 삭제, 검색 등의 처리를 요구하기 위해 사용하는 명령어
+ [[절차적 데이터 조작어(procedural DML)]]
+ [[비절차적 데이터 조작어(nonprocedural DML)]]

[[SQL(Structured Query Language)]]에서 테이블에 데이터를 검색, 삽입, 수정, 삭제하는 명령어

## SQL문 예시
---
명령어 문법은 모두 [[MySQL]]의 문법에 기초하였다
1. 데이터 검색: SELECT
 ```sql
 SELECT [ALL|DISTINCT] attribute_list FROM table_list [WHERE conditions] 
	 [GROUP BY attribute_list [HAVING condition]]
	 [ORDER BY attribute_list [ASC|DESC]];
 --example
 SELECT 제품명, 단가 AS 가격 FROM 제품;
 SELECT 제품명, 단가 '가 격' FROM 제품;
 SELECT 제품명, 단가 + 500 AS '조정 단가' FROM 제품;
 SELECT 주무제품, 수량, 주문일자, 주문 고객 FROM 주문 WHERE 주문고객='apple' OR 수량 >= '15';
 SELECT 고객이름, 나이, 등급, 적립금 FROM 고객 WHERE 고객이름 LIKE '김%';
 SELECT 고객이름 FROM 고객 WHERE 나이 IS NULL;
 SELECT 주문고객, 주문제품, 수량, 주문일자 FROM 주문 WHERE 수량 >= 10 
	 ORDER BY 주문제품 ASC, 수량 DESC; 
 SELECT AVG(단가) FROM 제품;            SELECT COUNT(*) AS 고객수 FROM 고객;
 SELECT 제조업체 COUNT(*) AS 제품수, MAX(단가) AS 최고가 FROM 제품 
	 GROUP BY 제조업체 HAVING COUNT(*) >= 3;
 ```
   + `SELECT` 키워드와 함께 검색하고 싶은 속성의 이름 나열
	   + 속성의 이름과 산술 연산자를 통해 계산된 결과값을 보여줄 수 있음(원본 값 변경x)
   + `FROM` 키워드와 함께 검색하고 싶은 속성이 있는 테이블의 이름을 나열
   + `WHERE` 키워드와 함께 검색 조건을 제시해 그 조건에 해당하는 투플만 나열 가능
   + `ALL`: 결과 테이블이 투플의 중복을 허용하도록 지정
   + `DISTINCT`: 결과 테이블이 투플의 중복을 허용하지 않도록 지정
   + `AS` 키워드: 이용해 결과 테이블에서 속성의 이름을 바꾸어 출력 가능 (새로운 이름에 공백이 포함된 경우 따음표를 통해 입력해야함), 생략 가능
   + `LIKE` 키워드: 부분적으로 일치하는 데이터 검색 (%: 0개 이상의 문자, $\_$: 1개 문자)
   + `IS NULL` 키워드: 특정 속성 값이 `NULL`인지 비교 가능
   + `ORDER BY` 키워드: 결과 테이블의 내용을 원하는 순서로 출력 가능
	   + `ASC`: 오름차순 $\qquad$ `DESC`: 내림차순
	   + `NULL` 값은 오름차순에서 맨 마지막에, 내림차순에선 맨 먼저 출력
	   + 여러 기준을 추가하려면 정렬 기준이 되는 속성들을 차례대로 제시
   + 집계 함수(arrregate function): 특정 속성 값을 통계적으로 계산한 결과 출력
	   + `COUNT`: 값의 개수$\quad$`MAX`: 최댓값$\quad$`MIN`: 최솟값$\quad$`SUM`: 합계$\quad$`AVG`: 평균
	   + `NULL` 값을 제외하고 계산하며 `SELECT` 절이나 `HAVING` 절에서만 가능
   + `GROUP BY` 키워드: 특정 속성 값이 같은 투플을 모아 그룹을 만들고 그룹별로 검색 가능
	   + `HAVING` 키워드와 함께 그룹에 대한 조건 나열 가능
	   + 그룹을 나누는 기준이 되는 속성을 `SELECT` 절에도 작성하는 것이 좋음
   + 여러 테이블에 대한 조인 검색도 가능
	   + 연결하려는 테이블 간에 조인 속성의 이름은 달라도, 도메인이 같아야 함.
	   + 일반적으로 외래키를 조인 속성으로 이용
```
```

2. 데이터 삽입: INSERT
```sql
 INSERT INTO table_name VALUES (attribute_value_list);
 ```
3. 데이터 수정: UPDATE
	
4. 데이터 삭제: DELETE

