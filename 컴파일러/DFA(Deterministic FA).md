
다음 5가지 요소로 정의된다.
$$DFA \; M = (Q, \Sigma, \delta, q_0, F)$$
1. $Q$: finite, non-empty set of states.
2. $\Sigma$: finite input alphabet
3. $\delta$: mapping function $$\delta = Q \times \Sigma \rightarrow Q$$
4. $q_0 \in Q$: start(or inital) symbol
5. $F \subseteq Q$: set of final states. 


$\delta(q, a)=p$ 인  [[FA(Finite Automata)]]를 말한다. 여기서 실제로 $p$가 아니라 $\{p\}$로 표기함이 옳지만, DFA인 경우에는 위와 $p$로 표기할 수 있다. 

DFA M에 의해 인식되는 [[String(Sentence)]] 전체를 모아 놓은 집합을 M에 의해 인식되는 [[언어(Language)]]라고 말하며, L(M)으로 표기한다. $$L(M) = \{x | \delta(q_0, x) \in F, \; x \in \Sigma^*\}$$
