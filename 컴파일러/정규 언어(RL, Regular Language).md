[[정규 문법(RG, Regular grammar)]]으로부터 생성된 [[언어(Language)]]이다.

주로 [[토큰(Token)]]의 구조를 정의하는데 사용된다. 이 이유는 다음과 같다.
+ [[토큰(Token)]]의 구조는 간단하기 때문에 [[정규 문법(RG, Regular grammar)]]으로 표현할 수 있다
+ [[CFG(Context-free grammar)]]보다는 [[정규 문법(RG, Regular grammar)]]으로부터 효율적인 [[인식기(Recognizer)]]를 구현할 수 있다
+ [[컴파일러(Compiler)]]의 Front-end를 모듈러하게 나누어 구성할 수 있다.

정규 언어는 3가지 표현을 통해 표현 될 수 있다.
1. [[정규 문법(RG, Regular grammar)]]
2. [[FA(Finite Automata)]]
3. [[정규 표현(RE, Regular Expression)]]


## **속성**

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

### [[정규 표현(RE, Regular Expression)]] $\Rightarrow$ [[FA(Finite Automata)]]
먼저 [[정규 표현(RE, Regular Expression)]]의 크기에 대해 다음과 같은 성질을 가지고 있다
+ The number of "state" is at most twice the size of the expression. Each operand introduces two states and each operator introduces at most two states. $|Q| \leq 2 \times size \; of \; RE$
+ The number of "arcs" is at most four times the size of the expression. $|\delta| \leq 4 \times size \; of \; RE$

이를 기본으로 다음과 같은 순서로 변환을 할 수 있다
1) RE $\Rightarrow$ $\epsilon-$NFA 
2) $\epsilon-$NFA $\Rightarrow$ DFA

RE의 basis인 $\epsilon$과 $a$는 다음과 같이 변환될 수 있다
+ $\epsilon: \; \delta(A, \epsilon) = B$
+ $a \in \Sigma: \; \delta(A, a) = B$


## **RL의 닫힘 성질(Closure properties of RL)**
If $L_1$ and $L_2$ are regular languages, then so are 
1) $L_1 \cup L_2$
2) $L_1 \cdot L_2$
3) $L_1^*$
> (proof) Since $L_1$ and $L_2$ are RL, $\exists RG \;G_1 = (V_{N1}, V_{T1}, P_1, S_1)$ and $G_2 = (V_{N2}, V_{T2}, P_2, S_2)$, such that $L(G_1) = L_1$ and $L(G_2) = L_2$
> 			$\quad$(ii) Construct $G = (V_{N1} \cup V_{N2}, V_{T1} \cup V_{T2}, P, S_1)$ in which $P$ is defined as follows: 
> 				$\quad\quad$	 $if \; A \rightarrow aB \in P_1, \; A \rightarrow aB \in P$
> 				$\quad\quad$	 $if \; A \rightarrow a \in P_1, \; A \rightarrow aS_2 \in P$
> 				$\quad\quad$All productions in $P_2$ are in $P$
> 				 $\quad\quad$We must prove that $L(G) = L(G_1)\cdot L(G_2)$
> 				 $\quad\quad$Since $G$ is RG, $L(G)$ is RL. Therefore $L(G_1)\cdot L(G_2)$ is RL.
> 			$\quad$(iii) Let $G' = (V_{N1} \cup \{ S' \}, V_{T1}, P', S')$
> 				$\quad\quad$  $P':$ $if \; A \rightarrow aB \in P_1, \; A \rightarrow aB \in P'$
> 				$\quad\quad$		 $if \; A \rightarrow a \in P_1, \; A \rightarrow a, \; A \rightarrow aS' \in P'$
> 				$\quad\quad$		 $S' \rightarrow S | \epsilon \in P'$
> 				$\quad\quad$ We must prove that $L(G') = (L(G))^*$
> 				$\quad\quad$ $\forall \omega \in L(G), \; S \Rightarrow^* \omega. \; S' \Rightarrow S \Rightarrow^* \omega S' \Rightarrow^* \omega^* S' \Rightarrow \omega^*$
> 				$\quad\quad$ $\therefore \; (L(G))^* = L(G')$
> 				$\quad\quad$ example) $P: S \rightarrow aS, \;S\rightarrow b$
> 				$\quad\quad\quad\quad\quad\quad\;$ $P': S\rightarrow aS, \; S \rightarrow b, \; S\rightarrow bS', \; S' \rightarrow S, \; S' \rightarrow \epsilon$
> 				$\quad\quad\quad\quad\quad\quad\;$note: $P: S = aS + b = a^*b$
> 				$\quad\quad\quad\quad\quad\quad\;$$P': S = aS + b + bS' = a^*(b+bS') = a^*b + a^*bS'$
> 				$\quad\quad\quad\quad\quad\quad\;$$\therefore S'= S + \epsilon = a^*bS'+a^*b+\epsilon = (a^*b)^*(a^*b+\epsilon)= (a^*b)^*(a^*b) + (a^*b)^* = (a^*b)^*$


## The Pumping Lemma for RL
주어진 [[언어(Language)]]가 [[정규 언어(RL, Regular Language)]]가 아님을 증명할 때 유용한 정리

> Let $L$ be a RL. There exists a constant $p$ such that if a string $\omega$ is in $L$ and $|\omega| \geq p$, then $\omega$ can be written $xyz$, where $0 \leq |y| \leq p$ and $xy^iz \in L \; for \; all \; i \geq 0$


