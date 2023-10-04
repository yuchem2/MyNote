[[정규 문법(RG, Regular grammar)]]으로부터 생성된 [[언어(Language)]]이다.

주로[[토큰(Token)]]의 구조를 정의하는데 사용된다. 이 이유는 다음과 같다.
+ [[토큰(Token)]]의 구조는 간단하기 때문에 [[정규 문법(RG, Regular grammar)]]으로 표현할 수 있다
+ [[CFG(Context-free grammar)]]보다는 [[정규 문법(RG, Regular grammar)]]으로부터 효율적인 [[인식기(Recognizer)]]를 구현할 수 있다
+ [[Compiler]]의 Front-end를 모듈러하게 나누어 구성할 수 있다.

정규 언어는 3가지 표현을 통해 표현 될 수 있다.
1. [[정규 문법(RG, Regular grammar)]]
2. [[FA(Finite Automata)]]
3. [[정규 표현(RE, Regular Expression)]]


## **속성**
---
정규 언어는 위와 같이 3가지 표현을 통해 표현될 수 있고, 각각의 표현을 다른 표현으로 쉽게 바꿀 수 있다.

### [[정규 문법(RG, Regular grammar)]] $\Rightarrow$ [[FA(Finite Automata)]]
Given RG, $G=(V_N, V_T, P, S)$, construct $M=(Q, \Sigma, \delta, q_0, F)$
1) $Q = V_N \cup \{f\}$, where $f$ is a new final state
2) $\Sigma = V_T$
3) $q_0 = S$
4) $F = \{f\} \; if \; \epsilon \notin L(G)$
        $= \{S, f\} \; otherwie$
5) $\delta: \; if \; A \rightarrow aB \in P \; then \; \delta(A, a) \ni B$
          $if \; A \rightarrow a \in P \; then \; \delta(A, a) \ni f$
> (proof) If $\omega$ is accepted by $FA$ then it is accepted in some sequence of moves through states, ending inf $f$.
> But if $\delta(A, a) = B$ and $B \neq f$, then $A \rightarrow aB$ is a productions.
> Also if $\delta(A, a) = f$ then $A \rightarrow a$ is a prodcution
> So we can use the same series of productions to generate $\omega$ in $G$
> 	$\therefore \; S \Rightarrow^* \omega$
> 	

### [[FA(Finite Automata)]] $\Rightarrow$ [[정규 문법(RG, Regular grammar)]]
Given $M=(Q, \Sigma, \delta, q_0, F)$, construct $G=(V_N, V_T, P, S)$
1) $V_N = Q$
2) $V_T = \Sigma$
3) $S = q_0$
4) $P: \; if \; \delta (q, a) = r \; then \; q \rightarrow ar$
           $if \; p \in F \; then \; p \rightarrow \epsilon$