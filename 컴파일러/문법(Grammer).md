
다음 네 가지 요소로 정의된다.

$$G = (V_N, V_T, P, S)$$

1. $V_N$: nonterminal symbol의 유한 집합
2. $V_T$: terminal symbol의 유한 집합$$V_N\; \cap \; V_T = \varnothing, \quad V_N \; \cup \; V_T = V $$
3. $P$: 생성 규칙(production rule)의 유한 집합. 각 생성 규칙의 형태는 아래와 같다. $$\alpha \rightarrow \beta, \quad \alpha \in V^+, \; \beta \in V^*$$
4. $S$: $V_N$에 속하는 심볼로서 다른 nonterminal symbol과 구별하여 시작 심벌 혹은 문장 심벌이라 한다.
5. $V$는 grammar symbol이라고 하며 noterminal symbol과 terminal symbol의 합집합이다. 


엄밀하게 $V_N, V_T, P, S$가 모두 정의되어야 문법이라고 한다. 

하지만, 암묵적으로 [[정규 문법(General Grammar)]]에서 $P$ 만 존재하는 경우 맨 처음 등장하는 lhs에 위치한 심볼을 $S$로 인지하고, 대문자 심볼을 $V_N$, 소문자 심볼을 $V_T$라고 판단한다. 

## **[[Language]]와의 관계**

+ [[문법(Grammer)]]으로부터 “유일”한 [[Language]]이 생성된다
	+ [[Derivation]]을 통해 생성되는 [[Language]]를 유추할 수 있다 
+ [[Language]]로부터 설계된 [[문법(Grammer)]]은 “다양한” 형태를 가질 수 있다
	+ 이렇게 설계할 수 있는 [[문법(Grammer)]] 중 가장 효율적인 형태를 구하는 것이 [[Automata]]의 목표


## **종류**

+ 



