
## [[이산수학]]에서의 정의
---
> A relation $R$ from a set $X$ to a set $Y$ is a subset of Cartessian product $X \times Y$. If $(x, y) \in R$, we write $xRy$, and say that x is related to y
> The domain of R is the set $Dom(R) = \{x \in X | (x, y) \in R \; for \; some \; y \in Y\}$
> The range of R is the set $Rng(R) = \{y \in Y | (x, y) \in R \; for \; some \; x \in X\}$


## [[데이터베이스(DB)]]에서의 정의
---
>Formally, given sets $D_1, D_2, ..., D_n$ a relation $r$ is a subset of $D_1 \times D_2 \times ... \times D_n$ (where $\times$ is Cartesian product) 
>Thus, a relation a set of n-tuples ($a_1, a_2, ..., a_n$) where each $a_i \in D_i$

+ 행과 열로 구성된 테이블을 의미한다. 
+ 하나의 개체에 관한 [[데이터(Data)]]를 2차원 테이블의 구조로 저장한 것이다. 
+ [[파일 시스템]] 관점에서 파일에 대응된다. 

### **구성**
---
+ [[차수(Degree)]]
+ [[카디널리티(cardicality)]]
+ 널(null)
+ [[릴레이션 스키마(Relation schema)]]
+ [[릴레이션 인스턴스(Relation instance)]]


### **특징**
---
+ [[속성(attribute)]]은 단일 값을 가진다
+ [[속성(attribute)]]은 서로 다른 이름(Specific identifier)을 가진다
+ 한 [[속성(attribute)]]에 속한 열은 모두 그 속성에서 정의한 [[도메인(domain)]] 값만 가질 수 있음
+ [[속성(attribute)]]의 순서가 달라도 [[릴레이션 스키마(Relation schema)]]는 같다
+ 중복된 [[투플(tuple))]]은 허용하지 않음([[키(Key)]]를 통해 이 특징 유지)
+ [[투플(tuple))]]의 순서는 상관없다 