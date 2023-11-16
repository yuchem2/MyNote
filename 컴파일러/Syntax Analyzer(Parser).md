## Concepts
---
Parser라고도 하며, 한국어로는 구문 분석기라고 한다.

Sysntax Checking과 [[트리(Tree)]] gereation 기능을 수행한다.

입력으로 일련의 [[토큰(Token)]]을 입력받아 오류가 있는지 확인한다. 오류가 있는 경우 error message를 출력하고, 오류가 없는 경우 parse tree와 parse라는 결과값을 내보낸다. 

[[Parsing Table]]을 통해 행동(Shift, Reduce, Accept, Error)를 결정한다
## Output of Parser
---
입력으로 스트링 $\omega$를 받아 만일 $\omega$가 정의된 문법의 문장이라면 구문 분석 정보를 생성하고 $\omega$가 정의된 문법의 문장이 아니라면 에러 메시지를 출력하는 작업을 수행한다. 올바른 문장에 대해서는 다음 단계에 필요한 구문 분석 정보를 내보내는데, 이 출력이 [[Intermediate Code Generator]]의 입력이 된다.
### Parse
유도 과정에서 적용된 일련의 생성 규칙 번호
#### Left Parse
좌측 유도를 적용한 생성 규칙 번호의 순열
e.g. $E\Rightarrow_1 E+T \Rightarrow_2 T+T \Rightarrow_4 F+T \Rightarrow_6 A+T \Rightarrow_3 A+ T*F \Rightarrow_4 a+F*F \Rightarrow_6 a+a*F \Rightarrow_6 a+a*a$
$\therefore 1 \; 2 \; 4\; 6\;3\; 4\; 6\; 6$   
#### Right Parse
우측 유도를 적용한 생성 규칙 번호의 역순
e.g. $E\Rightarrow_1 E+T \Rightarrow_3 T+T*F \Rightarrow_6 T+T*a \Rightarrow_4 E+F*a \Rightarrow_6 E+a*a \Rightarrow_2 T+a*a \Rightarrow_4 F+a*a \Rightarrow_6 a+a*a$
$\therefore 6 \; 4 \; 2\; 6\;4\; 6\; 3\; 1$   
### Parse tree
올바른 문장에 대해 구문 분석기가 그 문장의 구조를 트리 형태로 나타낸 것으로, 주어진 스트링에 대해 형태가 동일하며 유일하다. 하지만 적용된 구문 분석 방법에 따라 구성되는 순서가 다르다. Top-down 방식에서는 루트 노드로부터 생성 규칙이 적용되어 직접 유도될 때마다 트리가 구성되며 bottom-up 방식에서는 주어진 문장으로부터 생성 규칙이 reduce될 때마다 서브트리가 만들어진다. 

주어진 스트링을 구문 분석하여 구성된 파스 트리의 형태가 두 개 이상 생기는 경우 주어진 스트링을 결정적으로 구문 분석할 수 없다는 것을 의미한다. 
![[CFG(Context-free grammar)#Derivation Tree]]
#### Representation of a parse tree
+ Implicit representation: [[Derivation]]에서 사용된 생성 규칙의 순서
+ Explicit representation: [[연결 리스트(Linked List)#Doubly Linked List]] 자료구조 사용. 이때 좌측 pointer는 자식을, 우측 pointer는 형제 노드를 가리키게 구성
### AST(Abstract Syntax Tree)
Parse tree가 문법 구조에 관한 정확한 정보를 갖고 있으나 중간 노드에서 표현되는 nonterminal은 문법적인 정보만을 위한 것이므로, [[Intermediate Code Generator]]에서는 아무 의미가 없고 시간의 지연과 트리가 차지하는 기억 공간의 낭비만 초래한다. 그래서 Parse tree에서 불필요한 정보를 제거한 트리가 더 효과적이다. 

추상 구문 트리(AST)는 코드 생성 단계에서 필요한 의미 정보(semantic information)만을 갖는 트리. 즉, parse tree의 변환된 형태로, 소스 프로그램에 대한 의미 정보를 훨씬 효율적으로 나타내는 트리이다. 
+ Leaf node: operand(identifer or constant)
+ Internal node: operator(meaningful production rule name)

여기서 의미있는 생성 규칙은 compiler deginer가 지정하게 된다. 
## Syntax Analysis Method
---
[[CFG(Context-free grammar)]]를 위한 구문 분석 방법은 parse tree를 어떤 순서로 만들어 가느냐에 따라 크게 나누어 top-down 방식과 bottom-up 방식으로 구분할 수 있다. 
### Top-down
> Begining with the start symbol of the grammer, it attempts to produce a string of terminal symbol that is *identiacal* to a given source string. This matching process proceeds by successivlely applying the productions of the grammar to produce substrings from nonterminals

루트 노드로부터 시작하여 terminal node로 만들어가는 방식. 좌측 유도 순서를 따른다. 이 방식을 통해 만드는 parser의 종류는 다음과 같다.
+ recursive descent parser
+ predictive parser

Top-down parsing 방법은 다음과 같은 방법이 존재한다.
+ Parsing with backup or backtracking
+ Parsing with limited or partial packup
+ Parsing with nobracktracking
#### Genral Top-Down Parsing Method
Full backup 이나 backtracking을 이용한 방법으로, brute-force method라고도 불린다. 다음과 같은 과정을 통해 수행된다.
1. 처음에 시작 심벌의 첫 번째 생성 규칙을 적용하여 유도한다.
2. 주어진 입력 스트링과 유도된 문장 형태를 한번에 한 심벌씩 차례대로 비교한다.
3. 비교 과정에서 nonterminal이 나오며, 이 nonterminal 심벌의 첫 번째 생성 규칙을 적용하여 유도한다.
4. 비교한 두 심벌이 같으면 계속 비교하고, 다른 경우 현재 생성 적용한 생성 규칙을 제거하고, 다음 생성 규칙을 적용한다.
5. 2~4번 과정을 반복하다가 더 이상 적용할 생성 규칙이 없으면 입력 스트링을 틀린 문장으로 간주하고, 문장 형태에 나타난 스트링이 입력 스트링과 같아지면 올바른 문장으로 인식한다. 

##### 문제점
이 방법에서 다음과 같은 문제점이 존재한다. Left recursion이 존재하면 무한 루프에 빠지게 되며 결정적인 구문 분석을 하기 위해서는 backtracking을 하지 않아야 한다는 문제가 존재한다.
###### Left recursion
+ 어떤 $\alpha$에 대해 $A \Rightarrow^*Aa$의 유도 과정이 있으면 nonterminal $A$는 left recursion
+ left recursion한 nonterminal을 가지고 있는 문법은 left recursion
+ left recursion한 문법은 top-down parsing에서 무한 루프에 빠질 수 있다.
+ Direct left-recursion: $A\rightarrow A\alpha \in P$
+ Indirect left-recursion: $A\Rightarrow^+ A\alpha$
###### Backtracking
+ input string을 반복해서 scaning하며 stack 또한 다시 되돌려야 하는 소요가 발생
+ 이로 인해 parsing 속도가 매우 느려진다. 
##### 해결법
###### Left recursion
left recursion한 부분을 제거. 
```ad-note
title: Direct left-recursion
color: 127, 127, 127

일반적인 생성 규치 형태는 left recursion이 있는 생성 규칙과 없는 생성 규칙으로 구분해 $A\rightarrow A\alpha | \beta$로 나타낼 수 있다. 이 경우 nonterminal A가 생성하는 문장의 형태는 $\beta\alpha^*$이다. 따라서 동등한 언어를 생성하기 위해서는 $\beta\alpha^*$의 형태를 생성하면서 left-recursion을 갖지 않는 문자장으로 변환시켜야 한다. 

먼저 $\alpha^*$를 생성하는 문법은 새로운 nonterminal $A'$를 도입하여 다음과 같이 만들 수 있다. $$A'\rightarrow \alpha A' \;|\; \epsilon$$그러므로, $\beta\alpha^*$를 생성하는 문법은 다음과 같다. $$\begin{align}A& \rightarrow \beta A' \\ A' & \rightarrow \alpha A' \;| \;\epsilon\end{align}$$결국, left-recursion으로 되어 있던 생성 규칙을 right-recursion의 형태로 바꾼 결과가 되었다.

일반적으로 생성규칙에 나타나는 direct left-recursion을 제거하는 방법은 먼저 생성 규칙의 형태를 left-recursion이 있는 경우와 없는 경우로 나누어 정리한 후, $$A\rightarrow A\alpha_1 \; | \; A\alpha_2 \; | \; ... \; | \; A\alpha_m \; | \; \beta_1\; | \; \beta_2\;|\;...\;|\;\beta_n$$다음과 같이 변환하면 된다. $$\begin{align}A& \rightarrow \beta_1A' \; | \;\beta_2A'\;|\;...\;|\;\beta_nA'\\ A' & \rightarrow \alpha_1A' \;|\; \alpha_2A'\;|\; ... \;|\; \alpha_mA'\;| \;\epsilon\end{align}$$
```

```ad-note
title: Indirect left-recursion
color: 127, 127, 127

Indirect left-recursion은 먼저 direct left-recursion으로 형태로 고치고 direct left-recursion을 제거함으로써 해결할 수 있는데, 이를 pseudo code로 나타내면 다음과 같다.

	begin 
		nonterminal을 A1, A2, ..., An의 순서로 정돈한다.
		for i := 2 to n do
			for j := 1 to i-1 do
				각 Ai -> Aj r의 형태의 생성 규칙을 A_i -> a1 r | a2 r | ... | ak r로 대치한다.
				이때 Aj -> a1 | a2 | ... | ak는 모두 Aj의 생성규칙
			end for 
			Ai의 생성 규칙으로부터 직접 left-recursion을 모두 제거한다.
		end for
	end

이 방법을 보면, 변환하는 과정은 다음과 같다. 먼저 일정한 순서로 nonterminal을 순서화 한다. 그러면 생성 규칙에서 lhs보다 순서적으로 앞선 nonterminal이 rhs의 첫 번째로 나오는 생성 규칙에서 간접 순환이 발생한다. 따라서 이와 같은 형태의 생성 규칙을 대입 기법을 통해 제거한다. 
e.g. $S\rightarrow Aa\;|\;b \quad A \rightarrow Ac \;|\; Sd \;|\; e$
1. 대입 $A \rightarrow Ac \;|\; Aad \;|\; bd \;|\; e$
2. left-recursion 제거 $A \rightarrow eA' \;|\; bdA' \quad A' cA' \rightarrow cA'\;|\;adA'\;|\; \epsilon$
```
###### Backtracking
FIRST, FOLLOW를 이용하여 formal하게 정의
```ad-note
title: Left-factoring
color: 127, 127, 127

Top-down 구문 분석에서 같은 심벌들을 prefix로 갖는 두 개 이상의 생성 규칙이 있을 때 주어진 스트링이 올바른 문장인가를 검사하기 위하여 구문 분석기가 어떤 생성 규칙을 적용해야 할지 결정할 수 없다. 이러한 경우에 backtracking을 통해 구문 분석을 수행해야 하므로 비효율적이다. 따라서 구문 분석 결정 과정을 다음 심벌을 볼 때까지 연기함으로써 구문 분석기의 혼란을 막을 수 있다. 즉, 공통 앞부분을 새로운 nonterminal을 도입하여 인수 분해해야 한다. 

생성 규칙의 형태가 $A\rightarrow \alpha\beta \;|\;\alpha\gamma$인 경우에, 두 생성 규칙의 오른쪽 첫 심볼이 같은 스트링을 유도하므로 새로운 생성 규칙을 추가하여 다음과 같이 변환한다. $$\begin{align}A\rightarrow \alpha\beta\;|\;\alpha\gamma \Leftrightarrow A \rightarrow \alpha (\beta\;|\;\gamma) \Leftrightarrow & A\rightarrow \alpha A' \\ & A' \rightarrow \beta \;|\; \gamma\end{align}$$

이와 같은 과정을 left-factoring(좌 인수분해)이라고 한다. 
```

##### No-bracktracking
결정적으로 생성규칙을 적용할 수 있다는 것으로, 정의된 문법이 [[LL Condition]]을 만족하는 경우에 주어진 스트링을 결정적으로 구문 분석할 수 있다. 
### Bottom-up
terminal node로부터 루트 노드를 향하여 위로 만들어가는 방식. 우측 유도의 역순을 따른다. 이 방식을 통해 만드는 parser의 종류는 다음과 같다.
+ precedence pareser
+ shift-reduce parser

문장 형태에서 handle을 찾아 적용할 생성 규칙을 결정적으로 선택하는 것이 중심 측면이다. 
#### Reduce
>If $S\Rightarrow_{rm}^* \alpha\beta\omega, \; A\rightarrow \beta \in P$, then $S\Rightarrow_{rm}^* \alpha A\omega \Rightarrow_{rm} \alpha\beta\omega$.

특정 생성 규칙의 rhs가 문장 형태에 나타날 때 rhs를 lhs로 대치하는 것을 말함
#### Handle
>If $S\Rightarrow_{rm}^* \alpha\beta\omega$, then $\beta$ is a *handle* of $\alpha\beta\omega$
##### Handle pruning
>If $\exists S \Rightarrow_{rm} r_0 \Rightarrow_{rm} r_1 \Rightarrow_{rm} ... \Rightarrow_{rm} r_{n-1} \Rightarrow_{rm} r_n \Rightarrow_{rm} \omega$, then $\omega =\Rightarrow r_{n-1} =\Rightarrow r_{n-2} =\Rightarrow ... =\Rightarrow S$ is "reduce sequence"

#### Shift-reduce parsing
Automatic parsing에는 두 개의 문제점이 존재한다. 
+ 우측 문장 형태에서 handle을 어떻게 찾을 지
+ 같은 rhs에 대해 한 개 이상의 생성 규칙이 존재하는 경우 어떤 생성 규칙을 선택할 것인지

문법의 종류에 따라 방법이 결정되지만 handle을 유지하기 위해 stack을 사용한다. stack과 입력 버퍼를 사용하여 구현이 가능하다. stack은 보통 parsing stack이라 부르며 문장 형태에서 handle을 찾을 때가지 필요한 문법 심벌들을 유지하고 입력 버퍼는 주어진 스트링을 보유한다. 

stack top과 input symbol에 따라 parsing table을 참조하여 action을 결정한다. action은 총 4가지가 존재한다. 
1. shift: 다음 input symbol을 stack top에 shift
2. reduce: handle을 reduce해 생성규칙의 lhs로 변환
3. accept: parsing이 성공적으로 완료되었음을 알림
4. error: syntax error를 발견했음을 알리고, error recovery rountine을 호출
##### Thinking point
1. handle은 항상 stack top에 나타난다. 절대 stack 내부에서 등장해서는 안됨(우측 유도의 역순이기 때문)
2. 문법에 종류에 따라 만드는 방법이 다르다. 
	+ SLR(Simple LR)
	+ LALR(LookAhead LR)
	+ CLR(Canonical LR)
## Top-down Parser
---
### Deterministic Top-Down Parsing
---
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
![[LOOKAHEAD]]

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

## Bottom-up Parser
---