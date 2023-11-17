## Data Manipulation Language

[[데이터(Data)]]의 삽입, 삭제, 검색 등의 처리를 요구하기 위해 사용하는 명령어
+ [[절차적 데이터 조작어(procedural DML)]]
+ [[비절차적 데이터 조작어(nonprocedural DML)]]

[[SQL(Structured Query Language)]]에서 테이블에 데이터를 검색, 삽입, 수정, 삭제하는 명령어

## SQL문 예시
---
명령어 문법은 모두 [[MySQL]]의 문법에 기초하였다
### 데이터 검색: SELECT
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
#### 조인검색
여러 테이블에 대한 조인 검색도 가능
+ 연결하려는 테이블 간에 조인 속성의 이름은 달라도, 도메인이 같아야 함.
+ 일반적으로 외래키를 조인 속성으로 이용
+ `FROM` 절에서 검색에 필요한 모든 테이블을 나열
+ `WHERE` 절에 조인 속성이 같아야 함을 의미하는 조인 조건 제시
+ 표준 [[SQL(Structured Query Language)]]에서는 `INNER JOIN`, `OUTER JJOIN`과 `ON`키워드 제공
```sql
SELECT attribute_list FROM table1, table2 WHERE condition AND join_condition;
SELECT attribute_list FROM table1 INNER JOIN table2 ON join_condition 
	[WHERE condition];
SELECT attribute_list FROM table1 (LEFT | RIGHT | FULL) OUTER JOIN table2 
	ON join_condition [WHERE condition];
--example
SELECT 제품.주문명 FROM 제품, 주문 
	WHERE 주문.주문고객 = 'banana' AND 제품.제품번호 = 주문.주문제품;
SELECT 주문.주문제품, 주문.주문일자 FROM 고객, 주문
	WHERE 고객.나이 >= 30 AND 고객.고객아이디 = 주문.주문고객;
SELECT o.주문제품, o.주문일자 FROM 고객 c, 주문 o
	WHERE c.나이 >= 30 AND c.고객아이디 = o.주문고객;
SELECT 주문.주문제품, 주문.주문일자 
	FROM 고객 INNER JOIN 주문 ON 고객.고객아이디 = 주문.주문고객 
	WHERE 고객.나이 >= 30;
SELECT 고객.고객이름, 주문.주문제품, 주문.주문일자
	FROM 고객 LEFT OUTER JOIN 주문 ON 고객.고객아이디 = 주문.주문고객;
```
#### 부속 질의문
부속 질의문을 이용한 검색이 가능. `SELECT` 문 안에 다른 `SELECT` 문을 포함할 수 있음
+ 부속 질의문: 괄호로 묶어서 작성하며 `ORDER BY` 절을 사용할 수 없음
+ 부속 질의문을 먼저 수행하고,  그 결과를 이용해 상위 질의문 수행
+ 부속 질의문과 상의 질의문을 연결하는 연산자가 필요. 
	+ 단일 행 부속 질의문: `=, <>, >, >=, <, <=`
	+ 다중 행 부속 질의문: `IN, NOT IN, EXISTS, NOT EXISTS, ALL, ANY, SOME`
```sql
SELECT 제품명, 단가 FROM 제품 WHERE 제조업체 = (
	SELECT 제조업체 FROM 제품 WHERE 제품명 = '달콤비스킷'
);
SELECT 고객이름, 적립금 FROM 고객 WHERE 적립금 = (
	SELECT MAX(적립금) FROM 고객;
);
SELECT 제품명, 제조업체 FROM 제품 WHERE 제품번호 IN (
	SELECT 주문제품 FROM 주문 WHERE 주문고객 = 'banana'
);
SELECT 제품명, 단가, 제조업체 FROM 제품 WHERE 단가 > ALL (
	SELECT 단가 FROM 제품 WHERE 제조업체 = '대한식품'
);
SELECT 고객이름 FROM 고객 WHERE EXISTS (
	SELECT * FROM 주문 WHERE 주문일자='2022-03-15' AND 주문.주문고객 = 고객.고객아이디
);
```
#### 집합 연산
`UNION, MINUS, INTERSECT` 연산자를 이용해 집합 두 테이블에 대한 연산 가능
```sql
SELECT 주문고객 FROM 주문 WHERE 수량 >= 30 
	UNION SELECT 고객아이디 FROM 고객 WHERE 나이 >= 30;
-- mysql에는 MINUS 연산자가 없음
SELECT 주문고객 FROM 주문 WHERE 수량 >= 30 AND 주문고객 NOT IN (
	SELECT 고객아이디 FROM 고객 WHERE 나이 >= 30
);
-- mysql에는 INTERSECT 연산자가 없음
SELECT 주문고객 FROM 주문 WHERE 수량 >= 30 AND 주문고객 IN (
	SELECT 고객아이디 FROM 고객 WHERE 나이 >= 30
);
```
### 데이터 삽입: INSERT
```sql
 INSERT INTO table_name[(attribute_list)] VALUES (attribute_value_list);
 INSERT INTO table_name[(attribute_list)] SELECT ...; 
 --example
 INSERT INTO 고객(고객아이디, 고객이름, 나이, 등급, 직업, 적립금)
	 VALUES ('strawberry', '최유경', 30, 'vip', '공무원', 100);
 INSERT INTO 한빛제품(제품명, 재고량, 단가) 
	 SELECT 제품명, 재고량, 단가 FROM 제품 WHERE 제조업체='한빛제과';
```
+ `INTO` 키워드와 함께 투플을 삽입할 테이블의 이름가 속성의 이름을 나열
+ `VALUES` 키워드와 함께 삽입할 속성 값들을 나열
+ `INTO` 절의 속성 이름과 `VALUES` 절의 값은 순서대로 일대일 대응되어야 함
+ 속성 리스트를 나열한 경우 언급되지 않은 내용은 `NULL` 값으로 채워짐. (`NULL` 값을 허용하지 않는 속성은 무조건 `VALUES` 절에 입력해야 정상 입력)
### 데이터 수정: UPDATE
```sql
UPDATE table_name SET attribute_name1=value1, ... [WHERE condition];
UPDATE 제품 SET 제품명 = '통큰파이' WHERE 제품번호 = 'p03';
```
+ `SET` 키워드 다음에 속성 값을 어떻게 수정할 것인지를 지정
+ `WHERE` 절에 제시된 조건을 만족하는 투플만 속성 값을 수정
### 데이터 삭제: DELETE
```sql
DELETE FROM table_name [WHERE condition];
DELETE FROM 주문 WHERE 주문일자='2022-05-22';
```
+ `WHERE` 절에 제시한 조건을 만족하는 투플만 삭제. 생략하면 모든 투플 삭제
### [[뷰(View)]] 
![[뷰(View)]]
#### 뷰 생성: CREATE VIEW 
```sql
CREATE VIEW view_name[(attribute_name)] AS SELECT ... [WITH CHECK OPTION];
--example
CREATE VIEW 우수고객(고객아이디, 고객이름, 나이, 등급)
	AS SELECT 고객아이디, 고객이름, 나이, 등급 FROM 고객 WHERE 등급='vip'
	WITH CHECK OPTION;
CREATE VIEW 업체별제품수(제조업체, 제품수) 
	AS SELECT 제조업체, COUNT(*) FROM 제품 GROUP BY 제조업체
	WITH CHECK OPTION;
```
+ `CREATE VIEW` 키워드와 함께 생성할 뷰의 이름과 속성의 이름을 나열
+ `AS` 키워드와 함께 기본 테이블에 대한 `SELECT` 문 제시. 이때 `ORDER BY`는 사용 불가
+ `WITH CHECK OPTION`: 뷰에 삽입이나 수정 연산을 할 때 `SELECT` 문에서 제시한 뷰의 정의 조건을 위반하면 수행되지 않도록 하는 제약조건 제시
#### 뷰 활용: SELECT
+ 일반 테이블과 같은 방법으로 원하는 데이터를 검색할 수 있음
+ 뷰에 대한 `SELECT`문은 기본 테이블에 대한 `SELECT` 문으로 변환되어 수행
+ 모든 `SELECT` 연산이 가능
#### 뷰 활용: INSERT, UPDATE, DELETE
+ 뷰에 대한 삽입, 수정, 삭제 연산 가능. 실제로는 기본 테이블에 수행되므로 결과적으로는 기본 테이블이 변경
+ 변경 불가능한 뷰
	+ 기본 테이블의 기본키를 구성하는 속성이 포함되어 있지 않음
	+ 기본 테이블에서 `NOT NULL`로 지정된 속성이 포함되지 않음
	+ 기본 테이블에 있던 내용이 아닌 집계 함수로 새로 계산된 내용을 포함
	+ `DISTINCT` 키워드를 포함하여 정의됨
	+ `GROUP BY` 절을 포함하여 정의됨
	+ 여러 개의 테이블을 조인하여 정의한 경우는 불가능한 경우가 많음