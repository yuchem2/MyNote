[[인식기(Recognizer)]]의 한 종류로 Push-down list([[스택(Stack)]]), input tape, finite state control로 구성되어 있다.

7가지 요소로 정의된다. $$PDA \; P = (Q, \Sigma, \Gamma, \delta, q_0, Z_0, F)$$
1. $Q$: finite set of state symbols
2. $\Sigma$: finite set of input alphabets
3. $\Gamma$: finite set of stack symbols
4. $\delta$: mapping function $$\begin{align}\delta:\; &Q \times (\Sigma \cup \{\epsilon\}) \times \Gamma \rightarrow Q \times \Gamma^* \\ & \delta(q, a, Z) = \{(p_1, \alpha_1), (p_2, \alpha_2), ...,(p_n, \alpha_n)\}\end{align}$$
5. $q_0 \in Q$: start state
6. $Z_0 \in \Gamma$: start symbol of stack
7. $F \subseteq Q$: set of final state

여기서 mappping function $\delta$에 대해 현재 상태가 $q$이고, input symbol이 $a$이며 stack의 top symbol이 $Z$일 때, 다음 상태는 $n$개 중 하나로 변경되며 만약 ($p_i, \alpha_i$)가 선택되었다면 다음과 같이 말할 수 있다.
+ 현재의 $q$ 상태에서 입력 $a$를 본 다음 상태는 $p_i$이다
+ stack top symbol $Z$를 $\alpha_i$로 대치한다.

## Characteristic
---
+ P의 형태(configuration): 어떤 시점에서 P의 현재 상태를 나타내는 방법으로, $Q\times \Sigma^* \times \Gamma^*$인 $(q, \omega, \alpha)$로 표현한다. 여기서 $\alpha$를 서술할 때 stack의 top의 위치를 명시해야 한다. 
+ P의 이동(move): P에 의해 한 상태에서 다른 상태로의 이동은 $\vdash$로 표기한다. 여기서 [[Derivation]]과 같이 0번 이상의 이동을 $\vdash^*$로, 1번 이상의 이동을 $\vdash^+$로 표기한다.
	1) $a\neq \epsilon: \; (q, a\omega, Z\alpha) \vdash (q', \omega, \gamma \alpha)$ 
	2) $a=\epsilon:\; (q, \epsilon, Z) \vdash (q', \epsilon, \gamma) \Rightarrow \epsilon-move$ 
+ P의 시작 형태는 $(q_0, \omega, Z_0)$의 형태를 가지며 종결 형태는 $(q, \epsilon, \alpha), \; where \; q \in F, \;\alpha \in \Gamma^*$이다. 이때 P에 의해 accept되는 언어 $L(P)$는 다음과 같이 정의된다. $$L(P) = \{\omega\;|\;(q_0, \omega, Z_0) \;\vdash^*\; (q, \epsilon, \alpha),\; q\in F, \; \alpha\in \Gamma^*\}$$
## Extended PDA
---
원래 PDA에서 $\delta$의 정의가 변화된 형태로, 확장된 PDA에서 mapping function $\delta$는 다음과 같이 정의된다. $$\delta:\; Q \times (\Sigma \cup \{\epsilon\}) \times \Gamma^* \rightarrow Q \times \Gamma^*$$즉, 기존에는 한번의 move로 stack의 top symbol을 대치하였지만, 확장된 PDA에서는 한번의 move로 stack top 부분에 있는 유한 길이의 string을 다른 string으로 대치가 가능하다.$$(q, a\omega, \alpha\gamma) \vdash(q', \omega, \beta\gamma)$$또한, stack이 empty일 때도 move가 가능하다. 

확장된 PDA에 의해 accept되는 언어 $Le(P)$는 stack을 empty로 만드는 PDA에 의해 인식되는 string의 집합이며 다음과 같이 정의된다. $$Le(P) = \{\omega\;|\;(q_0, \omega, Z_0) \;\vdash^*\; (q, \epsilon, \epsilon),\; q\in Q\}$$
## PDA $\rightarrow$ extended PDA
---
두 언어는 모두 서로 변환이 가능하며 $Le(P') =L(P)$인 $P'$의 구성은 다음과 같다.
$P = (Q, \Sigma, \Gamma, \delta, q_0, Z_0, F) ===\Rightarrow P' =(Q\cup \{q_e, q'\}, \Sigma, \Gamma \cup \{Z'\}, \delta', q', Z', \varnothing),$
$\delta'$: 
1. 모든 $q \in Q, \;a \in \Sigma \cup \{\epsilon\}, \; Z \in \Gamma$에 대해 $\delta'(q, a, Z) =\delta(q, a, z)$
2. $\delta'(q', \epsilon, Z')=\{(q_0, Z_0Z')\}$. $Z'$: bottom marker
3. 모든 $q \in F, \; Z \in \Gamma \cup \{Z'\}$에 대해 $\delta'(q, \epsilon, Z)$에 $(q_e, \epsilon)$를 포함
4. 모든 $Z\in \Gamma\cup \{Z'\}$에 대해 $\delta'(q_e, \epsilon, Z) = \{(q_e, \epsilon)\}$

## [[CFL(Context Free Language)]]과 [[PDA(Pushdown Automata)]] 언어
---
PDA에 의해 accept되는 언어는 [[CFL(Context Free Language)]]이다. 즉, $L(CFG) = L(PDA)$
### [[CFG(Context-free grammar)]] $\rightarrow$ [[PDA(Pushdown Automata)]]
#### Top-Down Method
좌측 유도($A\Rightarrow^*\alpha$)를 사용. $CFG \;G$로부터 $Le(R)=L(G)$인 $PDA \;R$을 구성한다.
> For a given $G=(V_N, V_T, P, S)$, construct $R=(\{q\}, V_T, V_N \cup V_T, \delta, q, S, \varnothing)$
> where, $\delta$: 1) if $A\rightarrow\alpha\in P$, then $\delta(q, \epsilon, A)$에 $(q, \alpha)$를 포함 $\quad$ 2) $a\in V_T$에 대해 $\delta(q, a, a) = \{(q, \epsilon)\}$

이 방법으로 인식 과정을 보일 때 stack의 top은 왼쪽으로 해 기술한다.  
#### Bottom-up Method
우측 유도($\alpha=\Rightarrow^*A$)를 사용. 현재 대부분의 [[컴파일러(Compiler)]]가 채택하고 있는 방법. 오류 검출 속도가 빠르며 원인 확인이 쉬움. $CFG\; G$로부터 extended PDA를 구성
> For a given $G=(V_N, V_T, P, S)$, construct $R=(\{q, r\}, V_T, V_N \cup V_T \cup \{\$\}, \delta, q, \$, \{r\})$
> where, $\delta$: 1) if $\alpha \in V_T, \; \delta(q, \alpha, \epsilon)=(q, \alpha)$ $\quad$ 2) if $A \rightarrow \alpha \in P, \; \delta(q, \epsilon, \alpha) = (q, A)$ $\quad$ 3) $\delta(q, \epsilon, \$S)=(r, \epsilon)$ 

이 방법으로 인식 과정을 보일 때 stack의 top은 오른쪽으로 해 기술한다. 
### [[PDA(Pushdown Automata)]] $\rightarrow$ [[CFG(Context-free grammar)]]
$PDA \; P$로부터 $L(G)=Le(P)$인 $CFG\; G$를 구성한다.
> Given PDA $P = (Q, \Sigma, \Gamma, \delta, q_0, Z_0, F)$, construct $G=(V_N, V_T, P, S)$
> where, 1) $V_N = \{[qZr] \; | \; q, r \in Q, Z \in \Gamma\} \cup \{S\}\quad$ 2) $V_T = \Sigma \quad$ 3) $S$ is start symbol
> 4) $P$: (1) $\delta(q,a,Z)$가 $k\geq1$에 대해 $(r, X_1...X_K)$이면 $[qZs_k]\rightarrow a[rX_1s_1][s_1X_2s_2]...[s_{k-1}X_ks_k]$를 $P$에 추가. 이때 $s_1, s_2, ..., s_k \in Q$
> $\quad\quad\;$(2) $\delta(q, a, Z)$가 $(r, \epsilon)$를 포함하면 생성규칙 $[qZr]\rightarrow a$를 $P$에 추가 
> $\quad\quad\;$(3) 모든 $q\in Q$에 대해 $S\rightarrow[q_0Z_0q]$를 $P$에 추가