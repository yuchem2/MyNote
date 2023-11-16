유도 과정에서 나타난 문장 형태에서 nonterminal을 대치하기 위한 생성 규칙을 결정적으로 선택하기 위한 조건. 일반적으로 [[FIRST]]와 [[FOLLOW]]를 이용하여 정의한다. 
## LL condition
---
>For $\forall A\rightarrow \alpha \;|\; \beta \in P$, 
> 1. $FIRST(\alpha) \cap FIRST(\beta) = \varnothing$ 
> 2. if $\alpha \Rightarrow^* \epsilon(\epsilon \in FIRST(\alpha))$, then $FOLLOW(A) \cap FOLLOW(\beta) = \varnothing$

이때 $A\rightarrow \alpha \;|\; \beta \;|\; \gamma \in P$인 경우처럼 같은 lhs를 가지는 생성규칙들의 rhs는 위 조건에서 [[Mutually Disjoint]]해야 한다.  

LL condition을 만족하는 문법을 LL(1) 문법이라고 한다. 

## Strong LL condition
---
>For $\forall A\rightarrow \alpha \;|\; \beta \in P$, $$LOOKAHEAD(A\rightarrow\alpha) \cap LOOKAHEAD(A\rightarrow\beta) = \varnothing$$

같은 lhs를 가지는 별개의 생성규칙들 중에서 입력 심벌을 보고, 결정적으로 생성규칙을 선택할 수 있는 것을 의미한다. 

Strong LL condition을 만족하는 문법을 Strong LL(1) 문법이라고 하며 LL(1) 문법과 동일하다. 