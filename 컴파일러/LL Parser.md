### Deterministic Top-Down Parsing
Top-down 방법으로 구문 분석을 할 때 backtracking을 하지 않고 결정적으로 생성규칙을 선택해 적용하는 parsing을 말함. Input string을 한번만 스캐닝(left to right)을 하며 문장의 형태가 잘못됬다고 판단하면 바로 reject할 수 있다. *LL parsing*이라고도 부르며 LL은 "Left to right scanning and Left parse"의 약어이다.
#### [[FIRST]]
![[FIRST]]
#### [[FOLLOW]] 
![[FOLLOW]]
#### [[LL Condition]]
![[LL Condition#LL condition]]
### RDP(Recursive-Descent Parser)
> Recursive-descent parsing is a top-down method that uses a sef of *recursive procedures* to recognize its input with *no backtracking*

Parser를 일련의 순환 프로시저로 구현한 형태이다. 하지만 문법이 바뀌면 코드 자체를 전부 바꿔야 하는 소요가 있어 실제 [[컴파일러(Compiler)]] 구현에는 사용하지 않는 방법이다. 
#### LOOKAHEAD
Recursive-descent parser를 설계할 때 가장 중요한 것은 각 프로시저 내에서 입력 심벌에 따라 어떤 생성 규칙을 선택하느냐 하는 문제이다. 각 생성 규칙에 [[LOOKAHEAD]]를 계산함으로써 해결 할 수 있다.
![[LOOKAHEAD#LOOKAHEAD]]

RDP가 결정적으로 구문 분석하기 위해서는 [[LL Condition#Strong LL condition]]을 만족해야 한다. 
![[LL Condition#Strong LL condition]]
#### Implementation
If a grammar is strong LL(1), we can construct a parser for sentences of the grammar using the following scheme.
##### $\forall a \in V_T$
```c
pa() {
	if (nextSymbol == a)
		get_nextsymbol(); // get_nextsymbol() == scanner()
	else error();
}
```
##### $\forall A \in V_N$
```c
pA() {
	switch(nextSymbol) {
		case LOOKAHEAD(A -> X1X2...Xm): for i := 1 to m do pXi();
		case LOOKAHEAD(A -> Y1Y2...Yn): for i := 1 to n do pYi();
		                            ...
		case LOOKAHEAD(A -> Z1Z2...Zr): for i := 1 to r do pZi();
		case LOOKAHEAD(A -> e): ;
		default: error();
	}
}
```
#### Improving Implementation
1. Eliminating terminal procedures
2. [[BNF(Backus-Naur From)]] $\rightarrow$ [[EBNF(Extended BNF)]]: reduce the number of production and nonterminals

e.g. <IF_st>::= 'if' $<C>$ 'then'$<S>$$[$'else'$<S>$$]$
```c
pIF() {
	if (nextSymbol == 'if') {
		get_nextSymbol();
		pC();
		if (nextSymbol == 'then') {
			get_nextSymbol();
			pS();
		}
		else error(10); // 'then' error
	}
	else error(20); // 'if' error

	// optional
	if (nextSymbol == 'else') {
		get_nextSymbol();
		pS();
	}
}
```
### Predictive Parser
> Predictive parsing is a deterministic top-down parsing method using a stack. The stack contains a sequence of a grammar symbol.

RDP는 문법이 변경되면 프로그램을 수정해야 하는 단점이 존재했다. 이를 해결하기 위해 Predictive Parser가 등장하였다. 프로그램과 parsing table로 분리해 RDP의 단점을 극복하였다. 문법 변경 시 parsing table만 수정하면 된다.
#### Model
[[PDA(Pushdown Automata)]]에서 parsing table이 결합된 형태로 보면 된다. 
+ 현재 input symbol과 stack top symbol 사이의 관계에 따라 parsing을 진행
+ Initial configuration: STACK - $\$S$, $\quad$INPUT - $\omega\$$
+ Parsing table(LL): parsing action을 결정 $|V_N| \times (|V_T|+|\{\$\}|)$ 크기 
	+ `M[X, a] = r`: stack top symbol이 X이고, current symbol이 a일 때 r번 생성 규칙으로 expand
#### Parsing Action
1. if $X == a == \$$, then accept
2. if $X==a$, then pop $X$ and advance input
3. if $X \in V_N$, then if `M[x, a] = r`$(X\rightarrow \mu\upsilon\omega)$, then relace(expand) $X$ by $\mu\upsilon\omega$ else error
#### Algorithm
```pascal
set ip to point to the first symbol of w$;
	repeat
		let X be the top stack symbol and a the symbol pointed to by ip;
		if X is a terminal or $ then
			if X == a then
				pop X from the stack and adance ip
			else error(1)
		else
			if M[X, a] = X -> Y1Y2...Yk then
				begin pop X from the stack;
					push YkYk-1...Y1 onto the stack, with Y1 on top;
					output the production X -> Y1Y2...Yk
				end
			else error(2)
	until X == $ 
```
#### Parsing Table
> If $A \rightarrow \alpha$ is a production with $a$ in $FIRST(\alpha)$, then the parser will expand $A$ by $\alpha$ when the current input symbol is $a$. And if $\alpha \Rightarrow^* \epsilon$, then we should again expand $A$ by $\alpha$ when the current input symbol is in $FOLLOW(A)$

Parsing table(LL)에서 `M[X, a] = r`이면 r번 생성규칙으로 X을 expand. 비어있으면 error.
##### Algorithm for constructing a predictive parsing table
```pascal
begin
	for each production A -> alpha do
		for a in FIRST(alpha) do M[A, a] := A -> alpha;
		if a =>* e then for b in FOLLOW(A) do M[a, b] := A -> alpha;
	end for 
end 
```
##### LL(1) Grammar
>LL(1) Grammar is a grammar whose parsing table has no multiply-defined entries.(no collision)

### LL Grammar
#### Strong LL(k) and LL(k) Grammar
>Define $FIRST_k(\alpha) = \{\omega \;|\; \alpha \Rightarrow^* \omega \beta, |\omega|=k\; or \; \alpha \Rightarrow \omega \; and \; |\omega| < k\}$ and $k-LOOKAHEAD(A\rightarrow \alpha) = FIRST_k(\{\omega \;|\;S\Rightarrow^*\mu A\beta \Rightarrow \mu \alpha\beta \Rightarrow^* \mu\omega \in V_T^*\})$
##### Strong LL(k) Grammar
>G is said to be *strong LL(k)*, for some fixed integer k > 0, if whenever there are two leftmost derivations. $S\Rightarrow^* \mu A \gamma \Rightarrow \mu\alpha\gamma \Rightarrow^* \mu x \in V_T^*$and $S\Rightarrow^* vA\delta \rightarrow v\beta\delta \Rightarrow^* vy \in V_T^*$ such that $FIRST_k(X) = FIRST_k(y)$. It follows that $\alpha = \beta$. And it satisfies $$k-LOOKAHEAD(A\rightarrow\alpha) \cap k-LOOKAHEAD(A\rightarrow\beta) = \varnothing$$ for $\forall A \rightarrow \alpha \;|\; \beta \in P$
##### LL(k) Grammar
> G is said to be *LL(k)*, for some fixed integer k > 0, if whenever thear are two leftmost derivations.  $S\Rightarrow^* \mu A \gamma \Rightarrow \mu\alpha\gamma \Rightarrow^* \mu x \in V_T^*$and $S\Rightarrow^* \mu A\delta \rightarrow \mu\beta\delta \Rightarrow^* \mu y \in V_T^*$ such that $FIRST_k(X) = FIRST_k(y)$. It follows that $\alpha = \beta$