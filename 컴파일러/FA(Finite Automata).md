
다음 5가지 요소로 정의된다.

$$FA \quad M = (Q, \Sigma, \delta, q_0, F )$$
1. $Q$: finite, non-empty set of states.
2. $\Sigma$: finite input alphabet
3. $\delta$: mapping function $$\delta = Q \times \Sigma \rightarrow 2^Q (Power\; set \;of\; Q)$$
4. $q_0 \in Q$: start(or inital) symbol
5. $F \subseteq Q$: set of final states. 

최종 상태의 집합에 따라 [[DFA(Deterministic FA)]], [[NFA(Nondeterministic FA)]]로 구분한다.

유한 오토마타 M에 대해서 $if \; q \in Q \; and\; a \in \Sigma, \; \forall \delta (q, a)\; has \; exactly \; one \; number$ 인 경우에 M은 완전히 명시(completly specified)라고 하며 이러한 오토마타를 [[DFA(Deterministic FA)]]라고 한다. 

## **표현 방법**
---
1. $FA \; M = (Q, \Sigma, \delta, q_0, F )$
3. Transition table: FA의 전이함수 $\delta$를 matrix 형태로 나타낸 표
4. State transition diagram: FA의 각 상태를 노드로, 전이 함수를 directed arc로 표현한 그래프

![[Pasted image 20230926203841.png | 500]]

## **NFA를 DFA로 변환**
---
[[NFA(Nondeterministic FA)]]: 현실 세계의 정보를 묘사하기 쉬움
[[DFA(Deterministic FA)]]: 간단한 프로그램으로 시뮬레이션 하기 쉬움

위와 같은 특징을 가지고 있기 때문에 NFA를 설계한 뒤 DFA로 변환하고 이를 프로그램화하는 방식이 주로 사용된다.

NFA의 accepting sequence는 다음과 같다. 
$$ \begin{align}\delta(q_0, a_1a_2... a_n) & = \delta(\{q_1, q_2, ...,q_i\}, a_2a_3...a_n) \\
&= \delta(\{p_1, p_2, ..., p_j\}, a_3a_4...a_n) \\
&= \cdots = {r_1, r_2, ..., r_k} \end{align}$$
위 형식을 이용해 DFA의 상태를 NFA의 모든 상태의 부분집합으로 표현함으로 이 알고리즘은 *subsetset construction*이라고 한다.

>*Let L be a language accepted by NFA, then there exists DFA which accpets L.*
> (proof) Let $M=(Q, \Sigma, \delta, q_0, F)$ be a NFA accepting L.
> 	Define DFA $M' = (Q', \Sigma, \delta' q_0', F')$ such that
> 		(1) $Q' = 2^Q, \; {q_1, q_2, ..., q_i} \in Q', where \; q_i \in Q$
> 				denote a set of $Q'$ as $[q_1, q_2, ..., q_i]$
> 		(2) $q_0' = {q_0} = [q_0]$
> 		(3) $F'=\{[r_1, r_2, ..., r_k]\,|\,r_i \in F\,\}$
> 		(4) $\delta': \; \delta([q_1, q_2, ..., q_i], a) = [p_1, p_2, ..., p_j] \; if \; \delta(\{q_1, q_2, ..., q_j\}, a) = \{p_1, p_2, ..., p_j\}$
> 	Now we must prove that $L(M) = L(M')$ i.e, $$\delta'(q_0', x) \in F' \Leftrightarrow \delta(q_0, x) \cap F \neq \varnothing$$> 	(we can easily show that by *inductive hypothesis* on the length of the input string x)

임의의 $NFA \; M = (Q, \Sigma, \delta, q_0, F)$에 대하여 만들어지는 부분집합의 수는 $2^{|Q|}$개이다. 그러므로 너무 많은 상태가 만들어지며 그 중에는 시작 state에서 접근할 수 없는 상태(inaceessible state)도 존재한다. 

그러므로 일반적으로 도달 가능한 state에 대해서만 *subset construction*을 수행한다
> we call a state $p$ accessible if there is $\omega$ such that $(q_0, \omega) \Rightarrow^*(p, \epsilon)$, where $q_0$ is the initial state. 

