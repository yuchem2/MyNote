

$\forall\delta(q, a)=p$ 인  [[FA(Finite Automata)]]를 말한다. 여기서 실제로 $p$가 아니라 $\{p\}$로 표기함이 옳지만, DFA인 경우에는 위와 $p$로 표기할 수 있다. $$\delta = Q \times \Sigma \rightarrow Q$$Extension of $\delta$: $Q \times \Sigma^*$ $$\delta(q, \epsilon) = q, \quad \delta(q, xa) = \delta(\delta(q, x), a), where \; x \in \Sigma^* \; and \; a \in \Sigma$$

DFA M에 의해 인식되는 [[String(Sentence)]] 전체를 모아 놓은 집합을 M에 의해 인식되는 [[언어(Language)]]라고 말하며, L(M)으로 표기한다. $$L(M) = \{x | \delta(q_0, x) \in F, \; x \in \Sigma^*\}$$
단순히 입력 string이 끝날 때까지 mapping function을 일대일로 대응시키며 그 입력의 마지막 상태가 FA의 마지막 상태가 아니거나 중간에 접근불가능한 노드가 있는 경우에는 Rejected한다. 그러므로 **program으로 구현하기가 쉽다**

