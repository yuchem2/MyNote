
다음 5가지 요소로 정의된다.

$$FA \quad M = (Q, \Sigma, \delta, q_0, F )$$
1. $Q$: finite, non-empty set of states.
2. $\Sigma$: finite input alphabet
3. $\delta$: mapping function $$\delta = Q \times \Sigma \rightarrow 2^Q (Power\; set \;of\; Q)$$
5. $q_0 \in Q$: start(or inital) symbol
6. $F \subseteq Q$: set of final states. 

최종 상태의 집합에 따라 [[DFA(Deterministic FA)]], [[NFA(Nondeterministic FA)]]로 구분한다.