$\exists\delta(q, a)= {p_1, p_2, ..., p_n}$ 인  [[FA(Finite Automata)]]를 말한다. 즉, $\delta = Q \times \Sigma \rightarrow 2^Q$ 

Extension of $\delta$: $Q \times \Sigma^* \rightarrow 2^Q$ $$\begin{matrix} \delta(q, \epsilon) = \{q\} \\\\ \delta(q, xa) = \bigcup_{p \in \delta(q, x)} \delta(p, a), where \; x \in \Sigma^* \; and \; a \in \Sigma \\\\ \delta(\{p_1, p_2, ..., p_k\}, x) = \bigcup_{i=1}^k \delta(p_i, x)\end{matrix}$$
NFA M에 의해 인식되는 [[String(Sentence)]] 전체를 모아 놓은 집합을 M에 의해 인식되는 [[언어(Language)]]라고 말하며, L(M)으로 표기한다. $$L(M) = \{x | \delta(q_0m x)\, \cap \,F \neq \varnothing \}$$
임의의 string $w$가 NFA에 의해 accept되는지 확인하기 위해 단순히 state를 확인한다고 가정하고, 주어진 NFA가 m개의 state를 가진 FA라고 하자. 이런 경우에 최대 가능한 노드의 수는 ${|w|}^m$이 된다. 그러므로 **일반적으로NFA를 program으로 구현하기 어렵다**


$\epsilon$을 보고 갈 수 있는 mapping function이 있는 경우 $\epsilon$-NFA라고 하며 다음과 같이 정의한다.
$$\begin{align} \epsilon - NFA \quad M = (Q, \Sigma, \delta, q_0, F)  \\\\ 
\delta : Q \times (\Sigma \cup \{\epsilon\}) \rightarrow 2^Q\end{align}$$
여기서  $\epsilon$을 보고 갈 수 있는 상태들의 집합을  $\epsilon$-closure라고 하며 다음과 같이 정의한다.

$i)\;there \; is \; one \; state \; s$,
$$ \epsilon- closure(s)  = \{s\}\cup \{q|(p, \epsilon) = q, \;p \in \epsilon-closure(s)\}  $$
$ii) \; there \; is\; the\; set\; of\;  states$,
$$\epsilon- closure(T)   = \bigcup_{q \in T}\epsilon-closure(q)  $$
 