> $FIRST(\alpha) ::=$ the set of terminals that *begin* the strings derived from $\alpha$. If $\alpha \Rightarrow^* \epsilon$, then $\epsilon$ is also in $FIRST(\alpha)$$$FIRST(A)::=\{a \in V_T \cup \{\epsilon\}\;|\;A\Rightarrow^* a\alpha, \; \alpha \in V^*\}$$

※ nonterminal A가 $\epsilon$을 유도할 수 있는 경우 *nullable* 하다. ($A\Rightarrow^* \epsilon$) 
※ ring sum($\oplus$): 1. if $\epsilon \notin A$ then $A\oplus B =A \quad$ 2. if $\epsilon \in A$ then $A\oplus B = (A-\{\epsilon\} \cup B$  
## Computation
+ $FIRST(A_1A_2...A_n) = FIRST(A_1)\oplus FIRST(A_2)\oplus...\oplus FIRST(A_n)$
+ if $X \in V_T$, then $FISRT(X) =\{X\}$
+ if $X \in V_N$ and $X\rightarrow a\alpha \in P$, then $FIRST(X) = FIRST(X)\cup \{a\}$
+ if $X\rightarrow \epsilon \in P$, then $FIRST(X) = FIRST(X) \cup \{\epsilon\}$ 
+ if $X\rightarrow Y_1Y_2...Y_k \in P$, then $FIRST(X)=FIRST(X)\cup FIRST(Y_1Y_2...Y_k)$

문법 $G=(V_N, V_T, P, S)$가 주어졌을 때 FIRST를 구하는 알고리즘
```pascal
begin
	for each A in Vn do FIRST(A) := emptyset;
	for each A -> a in P do
		if a = aB, where a in Vt then
			FIRST(A) = FIRST(A) + {a}
			P := P - {A -> a}
		else if a = e then
			FIRST(A) = FIRST(A) + {e} fi
		fi
	end for
	repeat 
		for A -> Y1Y2...Yk in P, then
			FIRST(A) = FIRST(A) + (FIRST(Y1) (+) FIRST(Y2) (+) ... (+) FIRST(Yk))
		endfor
	until no change
end
```
여기서 (+)는 $\oplus$이고, +는 $\cup$, in은 $\in$, e는 $\epsilon$이다  