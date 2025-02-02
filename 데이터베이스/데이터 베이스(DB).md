
특정 조직의 여러 사용자가 공유하여 사용할 수 있도록 통합해서 저장한 운영 [[데이터(Data)]]의 집합

[[정보(Information)]] 시스템 안에 [[데이터(Data)]]를 저장, 제공하는 핵심 역할을 수행한다.

결국, [[정보처리(Information processing)]]를 위해 존재하는 시스템이다. 

## **개념**

1. Integrated data(통합된 데이터): 데이터를 통합하는 개념. 사용하던 데이터의 중복을 최소화하여 중복으로 인한 데이터 불일치 현상 제거 
2. Stored data(저장된 데이터): 컴퓨터 저장장치에 저장된 데이터를 의미
3. Operational data(운영 데이터): 조직의 목적을 위해 사용되는 데이터, 즉 업무를 위한 검색을 할 목적으로 저장된 데이터
4. Shared data(공유 데이터): 한 사람 또는 한 업무를 위해 사용되는 데이터가 아니라 공동으로 사용되는 데이터를 의미

## **특징**

1. 실시간 접근성(real time accessibility)
2. 계속적인 변화(continuous change): 저장된 내용은 한 순간의 상태를 나타내지만, 시간에 따라 변화
3. 동시 공유(concurrent sharing)
4. 내용에 따른 참조(reference by content): 데이터의 물리적인 위치가 아니라 데이터 내용에 따라 참조된다


## **분류**

1. [[정형 데이터(Structured data)]]
2. [[반정형 데이터(Semi-structured data)]]
3. [[비정형 데이터(Unstructured data)]]

## **발전**

1. [[파일 시스템]]
2. [[데이터 베이스 시스템]]
3. [[웹 데이터 베이스 시스템]]
4. [[분산 데이터 베이스 시스템]]

## **구성**

+ [[데이터 베이스 스키마(Schema)]]
+ [[데이터 베이스 인스턴스(instance)]]

## **개념적 구조**

[[데이터 베이스(DB)]]를 쉽게 이용할 수 있도록 하나의 데이터 베이스를 관점에 따라 세 단계로 나눈 것
+ [[외부단계(external level)]]: 개별 사용자 관점
+ [[개념 단계(conceptual level)]]: 조직 전체의 관점
+ [[내부 단계(internal level)]]: 물리적인 저장 장치의 관점

데이터 베이스를 3단계 구조로 나누고 단계별로 [[데이터 베이스 스키마(Schema)]]를 유지하여 [[데이터 베이스 스키마(Schema)]] 사이의 대응 관계를 정의하는 것이 궁극적인 목적이다  $\rightarrow$ 데이터의 독립서 실현


## [[데이버 베이스 설계]]
1) [[개념적 데이터 모델링]]: [[개체-관계 모델(E-R model; Entity-Relationship model)]] 설계
2) [[논리적 데이터 모델링]]: [[릴레이션 스키마(Relation schema)]] 설계
![[데이버 베이스 설계#Concepts]]
### [[데이터 정규화]]
![[데이터 정규화]]