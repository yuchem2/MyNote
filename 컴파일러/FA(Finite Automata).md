
다음 5가지 요소로 정의된다.

$$FA \quad M = (Q, \Sigma, \delta, q_0, F )$$
1. $Q$: finite, non-empty set of states.
2. $\Sigma$: finite input alphabet
3. $\delta$: mapping function $$\delta = Q \times \Sigma \rightarrow 2^Q (Power\; set \;of\; Q)$$
4. $q_0 \in Q$: start(or inital) symbol
5. $F \subseteq Q$: set of final states. 

최종 상태의 집합에 따라 [[DFA(Deterministic FA)]], [[NFA(Nondeterministic FA)]]로 구분한다.

유한 오토마타 M에 대해서 $if \; q \in Q \; and\; a \in \Sigma, \; \forall \delta (q, a)\; has \; exactly \; one \; number$ 인 경우에 M은 완전히 명시(completly specified)라고 하며 이러한 오토마타를 [[DFA(Deterministic FA)]]라고 한다. 

유한 오토마타를 표기하는 방법은 위와 같은 방법도 존재하지만 아래와 같은 방법도 존재한다.
1. Transition table: FA의 전이함수 $\delta$를 matrix 형태로 나타낸 표
2. State transition diagram: FA의 각 상태를 노드로, 전이 함수를 directed arc로 표현한 그래프

![[Pasted image 20230926203841.png | 500]]
