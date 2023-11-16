## Data Definition Language

[[데이터 베이스 스키마(Schema)]]를 정의하거나 수정 또는 삭제하기 위해 사용하는 명령어

[[SQL(Structured Query Language)]]에서 테이블이나 관계의 구조를 생성하는 명령어

## SQL문 예시
---
명령어 문법은 모두 [[MySQL]]의 문법에 기초하였다
1. 테이블 생성: CREATE TABLE 
```sql
CREATE TABLE table_name (
	(1) attribute_name data_type [NOT NULL] [DEFAULT default_value]
	(2) [PRIMARY KEY (attribute_list)]
	(3) [UNIQUE (attribute_list)]
	(4) [FOREIGN KEY(attribute_list) REFERENCES table_name(attribute_list)] 
		[ON DELETE options] [ON UPDATE options]
	(5) [CONSTRAINT name] [CHECK(conditions)]
);
```
   $[\;]$의 내용은 생략 가능하다 
   1) 테이블을 구성하는 각 [[속성(attribute)]]의 이름, 데이터 타입, 기본 제약 사항 정의
	   ※ data_type의 종류
	   + INT or INTEGER: 정수
	   + SMALLINT: INT보다 작은 정수
	   + CHAR(n) or CHARACTER(n): 길이가 n인 고정 문자열
	   + VARCHAR(n) or CHARACTER VARYING(n): 길이가 최대 n인 가변 문자열
	   + NUMERIC(p, s) or DECIMAL(p, s): 고정 소수점 실수, p는 정수부, s는 실수부 길이
	   + FLOAT(n): 길이가 n인 부동 소수점 실수
	   + REAL: 부동 소수점 실수
	   + DATE: 연, 월, 일로 표현되는 날짜
	   + TIME: 시, 분, 초로 표현되는 시간
	   + DATETIME: 날짜와 시간
   2) [[기본키(Primary key)]] 정의
   3) [[대체키(Alternate key)]] 정의: 유일성을 가지며 기본키와 달리 널 값이 허용
   4) [[외래키(Foreign key)]] 정의
	   ※ [[참조 무결성 제약조건]] 옵션
	   + ON DELETE NO ACTION: 투플을 삭제하지 못하게 함
	   + ON DELETE CASCADE: 관련 투플 함께 삭제
	   + ON DELETE SET NULL: 관련 투플의 외래키 값을 NULL로 변경
	   + ON DELETE SET DEFAULT: 관련 투플의 외래키 값을 미리 지정한 기본 값으로 변경
   5) [[데이터 무결성(Data Integrity)]]을 위한 제약조건 정의 
	   + 테이블에 정확하고 유요한 데이터를 유지하기 위해 특정 속성에 대한 제약조건 지정
	   + CHECK 키워드와 CONSTRAINT 키워드를 사용해 고유의 이름 부여 가능
<div></div>
```sql
...
CHECK (재고량 >= 0 AND 재고량 <= 1000)
CONSTRAINT CHK_CPK CHECK(제조업체='한빛제과')
...
```

2. 테이블 변경: ALTER TABLE
```sql
ALTER TABLE table_name ADD attribute_name data_type [NOT NULL] [DEFAULT default_value];
ALTER TABLE table_name DROP attribute_name data_type [NOT NULL] [DEFAULT default_value];
--example
ALTER TABLE table_name ADD CONSTRAINT condition_name conditions;
ALTER TABLE table_name DROP CONSTRAINT condition_name;
```
만약 삭제할 속성과 관련된 제약조건이나 참조하는 다른 속성이 존재하는 경우 삭제가 수행되지 않음. 관련된 제약조건이나 참조하는 다른 속성을 먼저 삭제해야 한다

3. 테이블 삭제: DROP TABLE
```sql
DROP TABLE table_name;
```
만약 삭제할 테이블을 참조하는 테이블이 존재하고 있다면 데이터 삭제가 수행되지 않는다. 먼저 외래키 제약조건을 삭제한 후에 삭제가 가능하다