> A regullar expression over the alphabet T and the language denoted by that expression are defined recursively as follow:
> 1. Basic Element: $\emptyset, \; \epsilon, \; a \in T$
> 	(1) $\emptyset$ is a regular expression denoting the empty set
> 	(2) $\epsilon$ is a regular expression denoting $\{ \epsilon \}$
> 	(3) $a$ where $a \in T$ is a regular expression denoting $\{ a \}$
> 2. Operator: $+, \; \cdot, \; *$
>	(1) $(P + Q)$ is a regular expression denoting  $L_p \cup L_q$
>	(2) $(P\cdot Q)$ is a regular expression denoting $L_p \cdot L_q$ ([[Language product]])
>	(3) $(P^*)$ is a regular expression denoting $$\{\epsilon\}\cup L_p \cup L^2_p \cup \; ... \; \cup L^n_p \cup ... $$
>		note: precedence : $+ < \cdot < *$
>3. Nothing else is a regular expression

[[정규 언어(RL, Regular Language)]]를 표현하기 위한 한 방법으로, [[정규 언어(RL, Regular Language)]]에 속해 있는 [[String(Sentence)]]의 모양을 직접 기술하는 형태를 가짐

### 추가 정리
+ If $\alpha$ is a regular expression then $L(\alpha)$ is the language denoted by $\alpha$. Let $\alpha$ and $\beta$ be regular expressions, then
	1. $L(\alpha + \beta) = L(\alpha) + L(\beta)$
	2. $L(\alpha\beta) = L(\alpha)L(\beta)$
	3. $L(\alpha^*) = L(\alpha)^*$
+ Two regular expressions are *equal* iff they denoted the same language. $$L(\alpha) = L(\beta) \Leftrightarrow \alpha = \beta $$
+ Axioms: Let $\alpha, \; \beta,$ and $\gamma$ be regular expressions. Then,
	1. $\alpha + \beta = \beta + \alpha$
	2. $(\alpha + \beta) + \gamma = \alpha + (\beta + \gamma)$
	3. $(\alpha\beta)\gamma = \alpha(\beta\gamma)$
	4. $\alpha(\beta + \gamma) = \alpha\beta + \alpha\gamma$
	5. $(\beta + \gamma)\alpha = \beta\alpha + \gamma\alpha$
	6. $\alpha + \alpha = \alpha$
	7. $\alpha + \emptyset = \alpha$
	8. $\color{orchid}\alpha\emptyset = \emptyset = \emptyset\alpha$
	9. $\color{orchid}\epsilon\alpha = \alpha = \alpha\epsilon$
	10. $\alpha^* = \epsilon + \alpha \cdot \alpha^*$
	11. $\alpha^* = (\epsilon + \alpha)^*$
	12. $(\alpha^* )^* = \alpha^*$
	13. $\alpha^* + \alpha = \alpha^*$
	14. $\alpha^* + \alpha^+ = \alpha^*$
	15. $\color{orchid}(\alpha+\beta)^* = (\alpha^*\beta^*)^*$ 

