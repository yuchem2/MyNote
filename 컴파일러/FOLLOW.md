> $FOLLOW(A)::=$ the set of terminals that can appear immediately to the right of  A in some sential form. If A can be the *rightmost symbol* in some sential form, then $ is in $FOLLOW(A)$. $ is the input right marker.$$FOLLOW(A) ::= \{a \in V_T \cup\{$\} \;|\;S \Rightarrow^* \alpha Aa \beta,\;\alpha,\;\beta \in V^*\}$$

## Computation
+ $FOLLOW(S) = \{\$\}$ where S is start symbol
+ if $A\rightarrow \alpha B \beta \in P$ and $\beta \neq \epsilon$, then $FOLLOW(B) = FOLLOW(B) \cup (FIRST(\beta) - \{\epsilon\})$
+ if $A \rightarrow \alpha B \in P$ or $A \rightarrow \alpha B \beta$  and $\beta \Rightarrow^* \epsilon$, $FOLLOW(B) = FOLLOW(B) \cup FOLLOW(A)$

문법 $G=(V_N, V_T, P, S)$가 주어졌을 때 FOLLOW를 구하는 알고리즘
```pascal
begin
	for each A in Vn do FOLLOW(A) := emptyset;
	FOLLOW(S) := {$};
	for A -> aBb in P where b != e do
		FOLLOW(B) = FOLLOW(B) + (FIRST(b) - {e})
	repeat
		for A -> a in P do
			if a = rB or (a = rBb and B =>* e) then
				FOLLOW(B) = FOLLOW(B) + FOLLOW(A)
			fi
		end for;
	until no change
end
```
여기서 +는 $\cup$, in은 $\in$, e는 $\epsilon$이다