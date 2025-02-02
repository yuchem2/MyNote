> A regullar expression over the alphabet T and the language denoted by that expression are defined recursively as follow:
> 1. Basic Element: $\varnothing, \; \epsilon, \; a \in T$
> 	(1) $\varnothing$ is a regular expression denoting the empty set
> 	(2) $\epsilon$ is a regular expression denoting $\{ \epsilon \}$
> 	(3) $a$ where $a \in T$ is a regular expression denoting $\{ a \}$
> 2. Operator: $+, \; \cdot, \; *$
>	(1) $(P + Q)$ is a regular expression denoting  $L_p \cup L_q$
>	(2) $(P\cdot Q)$ is a regular expression denoting $L_p \cdot L_q$ ([[Language product]])
>	(3) $(P^*)$ is a regular expression denoting $$\{\epsilon\}\cup L_p \cup L^2_p \cup \; ... \; \cup L^n_p \cup ... $$
>		note: precedence : $+ < \cdot < *$
>3. Nothing else is a regular expression

[[정규 언어(RL, Regular Language)]]를 표현하기 위한 한 방법으로, [[정규 언어(RL, Regular Language)]]에 속해 있는 [[String(Sentence)]]의 모양을 직접 기술하는 형태를 가짐

즉, [[토큰(Token)]]의 어휘 구조로, [[Lexical Analyzer(Scanner)]]의 출력이 된다.
## **추가 정리**
+ If $\alpha$ is a regular expression then $L(\alpha)$ is the language denoted by $\alpha$. Let $\alpha$ and $\beta$ be regular expressions, then
	1. $L(\alpha + \beta) = L(\alpha) + L(\beta)$
	2. $L(\alpha\beta) = L(\alpha)L(\beta)$
	3. $L(\alpha^*) = L(\alpha)^*$
+ Two regular expressions are *equal* iff they denoted the same language. $$L(\alpha) = L(\beta) \Leftrightarrow \alpha = \beta $$
+ *Axioms*: Let $\alpha, \; \beta,$ and $\gamma$ be regular expressions. Then,
	1. $\alpha + \beta = \beta + \alpha$
	2. $(\alpha + \beta) + \gamma = \alpha + (\beta + \gamma)$
	3. $(\alpha\beta)\gamma = \alpha(\beta\gamma)$
	4. $\alpha(\beta + \gamma) = \alpha\beta + \alpha\gamma$
	5. $(\beta + \gamma)\alpha = \beta\alpha + \gamma\alpha$
	6. $\alpha + \alpha = \alpha$
	7. $\alpha + \varnothing = \alpha$
	8. $\color{orchid}\alpha\varnothing = \varnothing = \varnothing\alpha$
	9. $\color{orchid}\epsilon\alpha = \alpha = \alpha\epsilon$
	10. $\alpha^* = \epsilon + \alpha \cdot \alpha^*$
	11. $\alpha^* = (\epsilon + \alpha)^*$
	12. $(\alpha^* )^* = \alpha^*$
	13. $\alpha^* + \alpha = \alpha^*$
	14. $\alpha^* + \alpha^+ = \alpha^*$
	15. $\color{orchid}(\alpha+\beta)^* = (\alpha^*\beta^*)^*$ 
+ Regular expression equations ::= the set of equation whose coefficient are regular expressions. $$if \; \alpha \; and \; \beta \; are \; regular \; expression, \; X = \alpha X + \beta \; is \; regular \; expression \; equations.$$
	(a) If $\epsilon$ is not in $\alpha$, then $X = \alpha^* \beta$ is the *unique* solution.
	(b) If $\epsilon$ is in $\alpha$, then $X = \alpha^* (\beta + L)$ for some Language $L$. 
	  Smallest solution:  $X = \alpha^* \beta$ 
+ The *size of a regular expression* is the number of operations and operands in the expression
	e.g. $size(ab+c^*) = 6$


## **실제 사용 예시**

+ 주어진 [[정규 문법(RG, Regular grammar)]] $G$가 생성하는 [[정규 언어(RL, Regular Language)]] $L$을 표현하는 [[정규 표현(RE, Regular Expression)]]을 구하는 방법은 다음과 같다.
	+ $L(A)$ where $A \in V_N$ denotes the language generated by A.
	+ By definition, if $S$ is a start symbol, then $L(G)=L(S)$
	1. Construct a set of simultaneous equations from $G$. $$if \; A \rightarrow aB, \; A \rightarrow a, \; L(A) = \{a\} \cdot L(B) \cup \{a\} \Rightarrow A = aB + a$$ In genearl, $X \rightarrow \alpha |\beta|\gamma \Rightarrow X = \alpha + \beta + \gamma$
	2. Solve these equations. $$X = \alpha X + \beta \Leftrightarrow X = \alpha^* \beta$$
