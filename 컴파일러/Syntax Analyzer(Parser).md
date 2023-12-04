## Concepts
Parser라고도 하며, 한국어로는 구문 분석기라고 한다.

Sysntax Checking과 [[트리(Tree)]] gereation 기능을 수행한다.

입력으로 일련의 [[토큰(Token)]]을 입력받아 오류가 있는지 확인한다. 오류가 있는 경우 error message를 출력하고, 오류가 없는 경우 parse tree와 parse라는 결과값을 내보낸다. 

[[Parsing Table]]을 통해 행동(Shift, Reduce, Accept, Error)를 결정한다
## Output of Parser
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
## LL Parser
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

## LR Parser
### Deterministic Bottom-up Parsing
Bottom-up parsing을 진행할 때 backtracking없이 구문 분석하는 방법을 말한다. 입력에 대해Left-to right scanning을 진행하고, 우측 유도를 이용해 파싱을 진행한다. LR은 Left to right scanning and Right parse의 약어이다. 

일반적으로 [[Syntax Analyzer(Parser)#LL Parser]]보다 많이 사용되는 형태인데, 이유는 다음과 같다. 
+ 대부분의 [[프로그래밍 언어]]를 구성할 수 있다.
+ LL parsing 방법보다 일반적인 방법이다.
+ syntatic error를 빠르게 탐지할 수 있다.

하지만 [[프로그래밍 언어]] 문법을 위한 LR parser로 직접 구현하는 것은 오랜 시간이 걸려, [[Parser Generating System]]을 자주 이용한다. 
### LR(0) items
문법으로부터 LR parsing table을 구성하기 위해 사용되는 방법에서 사용되는 요소. LR(0) item을 정의하기 전에 문법을 다음 방법으로 확장한다. 

![[Augmented Grammar]]
이러한 형태로 문법을 확장하는 이유는 parsing을 멈추고, 입력으로 주어진 string을 인식하는 시점을 결정하기 위해서다. 해당 생성 규칙을 추가하게 되면 string이 parser에 의해 accept되는 경우는 오직 $S'\rightarrow S$로 reduce될 때이다.  

LR(0) item은 다음과 같이 정의한다.
> LR(0) item is a production with a dot at some position of the right side. $$\begin{align}e.g. A\rightarrow XYZ \in P,\\&[A\rightarrow.XYZ]\; [A\rightarrow X.YZ]\\ & [A\rightarrow XY.Z] \; [A\rightarrow XYZ .]\end{align}$$

여기서 dot 이후에 처음 등장하는 symbol을 mark symbol이라고 하며 다음과 같이 정의한다. 
> A mark symbol is the symbol after the dot if it exists

LR(0) item은 dot의 위치에 따라 다음과 같이 구분할 수 있다. 
>+ kernel item $::= [A\rightarrow \alpha.\beta]\;if\;\alpha \neq \epsilon$
>+ closure item $::= [A\rightarrow .\alpha]$: the result of performing the CLOSURE operation, 
>  $\qquad\qquad\qquad\quad\;\;$ but $[S'\rightarrow .S]$ is kernel item
>+ reduce item $::=[A\rightarrow \alpha.]$: dot is at the end of rhs

여기서 $[A\rightarrow \alpha.\beta]$는 다음과 같은 의미를 가진다. 
+ $\alpha$로부터 유도된 입력 스트링들은 이미 확인되었다. (현재 stack의 top에 $\alpha$가 존재)
+ $\beta$로부터 유도될 수 있는 입력 스트링이 다음 확인에 등장한다면 $A\rightarrow \alpha \beta$에 의해 reduce한다.

viable prefix는 다음과 같이 정의한다.
> If $S'\Rightarrow_{rm}^* \alpha A \omega \Rightarrow_{rm} \alpha\beta_1 \beta_2 \omega$, then $\alpha\beta_1$ is a viable prefix. It is a prefix of a right sential form that does not continue past the right end of the handle of that sential form.
> 
> We say item $[A\rightarrow\beta_1.\beta_2]$ is valid for a viable prefix $\alpha\beta_1$ if there is a derivation $S'\Rightarrow_{rm}^* \alpha A \omega \Rightarrow_{rm} \alpha\beta_1 \beta_2 \omega, \; \omega \in V_T^*$.

#### CLOSURE operation
CLOSURE(I) 연산은 다음과 같이 정의한다. $$CLOSURE(I) = I \cup \{[B\rightarrow.\gamma]\; | \; [A\rightarrow \alpha.B\beta] \in CLOUSE(I), \; B\rightarrow\gamma \in P\}$$$[A\rightarrow \alpha.B\beta]$가 CLOSURE(I)에 속할 때 입력에서 $B$로부터 유도되는 문장 형태가 존재할 것이라고 기대할 수 있다. 이 경우에 만약 $B\rightarrow\gamma$라는 생성규칙이 존재하면, $\gamma$로부터 시작하는 문장 형태가 존재할 수 있다. 이러한 이유로, $[B\rightarrow.\gamma]$도 CLOSURE(I)에 포함시킨다. 
##### Computing Algorithm
```pascal
begin
	CLOSURE = I;
	repeat
		if [A -> a.Bb] in CLOSURE and B -> r in P then
			if [B -> .r] not in CLOSURE then
				CLOSURE := CLOSURE + [B -> .r]
	until no change
end
```
#### GOTO operation
GOTO(I, X) 연산은 다음과 같의 정의한다. $$GOTO(I, X) = CLOSURE(\{[A\rightarrow \alpha X.\beta] \; | \; [A\rightarrow \alpha.X\beta \in I\})$$즉, X가 mark symbol인 LR(0) item이 I에 속해 있는 경우 그 LR(0) item의 dot을 X의 뒤로 이동한 LR(0) item의 집합에 CLOSURE 연산을 수행한 결과이다. 
#### Canonical Collection
> Canonical collection of LR(0) items is the set of valid items for each viable prefix that can appear on the stack of an LR parser$$C_0 = \{CLOSURE(\{[S'\rightarrow.S]\})\} \cup \{GOTO(I, X) \; | \; I \in C_0, \; X \in V\}$$

일반적으로 주어진 문법의 $C_0$를 계산하기 위해 문법을 먼저 [[Augmented Grammar]]로 확장한 뒤에 연산을 수행한다. 
##### Computing Algorithm
```pascal
begin
	C0 := CLOSURE([S`-> .S])
	repeat
		for I in C0 do
			Closure = CLOSURE(I)
			for each X in mark symbol of Closure do
				J := GOTO(I, X)
				if Ji = J then GOTO(I, X) := Ji
				else GOTO(I, X) := J
				C0 := C0 + J
			end for
		end for
	until no change
end
```
### Parsing Table
행은 state로 구별되고 열은 ACTION 부분과 GOTO 부분으로 구성($|state|\times|V_T\cup \{\$\}\cup V_N|$)
ACATION 부분은 index로 terminal symbol과 $를 가지고 있으며 shift-reude parsing과 동일한 action을 수행한다. GOTO 부분은 nonterminal symbol을 index로 가진다. parsing table의 연산을 정리하면 다음과 같다.
+ ACTION$[S_m,a_i]$ = $shift\; S$ $::= (S_0X_1S_1 ...X_mS_m, a_ia_{i+1}...a_n\$) \Rightarrow(S_0X_1S_1X_mS_ma_jS, A_{i+1}...a_n\$)$ 
+ ACTION$[S_m, a_i]$ = $reduce \; A \rightarrow \beta$ and $|\beta| = r$ $::=(S_0X_1S_1 ...X_mS_m, a_ia_{i+1}...a_n\$) \Rightarrow (S_0X_1S_1 ... X_{m-r}S_{m-r}, a_ia_{i+1}...a_n\$), GOTO(S_{m-r}, A) = S$$\;\;\Rightarrow(S_0X_1S_1 ... X_{m-r}S_{m-r}AS, a_ia_{i+1} ... a_n\$)$ 
+ ACTION$[S_m, a_i]$ = $accept$, parsing 종료
+ ACTION$[S_m,a_i]$ = $error$, parser가 error를 감지. [[Error Recovery(Complier)]] routine 호출

parsing table을 구성하는 방법은 다음과 같은 3가지 방법이 존재한다. 
1. SLR(simple LR): $C_0$, [[FOLLOW]]를 기반으로 구성 
2. CLR(Canonical LR): $C_1$을 기반으로 구성
3. LALR([[LOOKAHEAD]] LR): $C_1$과 $C_0$ & [[LOOKAHEAD]]를 기반으로 사용

위 방법에서 만드러어진 상태 수는 다음과 같다.
1. SLR: $|V|\times |C_0|$
2. CLR: $|V|\times |C_1|$
3. SLR: $|V|\times |C_0$
### SLR Parsing Table
#### Construction
$C_0$를 계산한 후 다음과 같은 과정으로 테이블을 구성한다. 
1. $ACTION[i, a]:= shift \; j, \; if \; [A\rightarrow \alpha.a\beta]\in I_i\; and \; GOTO(I_i, a) = I_j$ 
2. $ACTION[i, a]:= \; reduce \; A\rightarrow \alpha, \; for\; all \; a \in FOLLOW(A) \; if\; [A\rightarrow\alpha.] \in I_i$
3. $ACTION[i,\$]:= accept\; if \; [S'\rightarrow S.] \in I_i$
4. $GOTO(i, A) := j \; if \; GOTO(I_i, A) = I_j$
5. error for all undefinded entries and initial state is $i \; if \; [S'\rightarrow.S]\in I_i$
#### Issues
SLR에서는 [[FOLLOW]]를 통해 reduce를 진행한다. 하지만 특정 상황에서 해당 terminal symbol을 보고 reduce를 못하는 경우가 생기기도 한다. 이로 인해 table을 구성할 때 충돌이 발생할 수도 있다. 

### LR(1) items
> LR(1) item ::= LR(0) + [[LOOKAHEAD]] $$[A\rightarrow\alpha.\beta, \;a] \; where \; A\rightarrow \alpha\beta \in P \; and \; a \in V_T \cup \{\$\}$$+ 1 is the length of the [[LOOKAHEAD]]
> + $A\rightarrow \alpha.\beta$ is called core
> + a is called the [[LOOKAHEAD]] of the item

[[LOOKAHEAD]]가 모든 LR(1) item에 나타나지만, [[LOOKAHEAD]]가 parsing table을 구성할 때 의미를 가질 때는 주어진 item이 reduce symbol일 때만 다음 심벌이 [[LOOKAHEAD]]인 경우에 reduce하라는 의미를 가진다. 

LR(1) item에서 Canonical Collection을 구하는 방법은 LR(0) item의 경우와 동일하지만 표기할 때는 $C_1$로 표기한다. 
#### CLOSURE operation
CLOSURE(I) 연산은 다음과 같이 정의한다. $$\begin{align}CLOSURE(I) = I \cup \{[B\rightarrow.\gamma, \;b] \;  |& \; [A\rightarrow\alpha.B\beta, \; a] \in CLOSURE (I), \\&\; B \rightarrow \gamma \in P, \; b \in FIRST(\beta a)\}\end{align}$$
### CLR Parsing Table
SLR를 구성할 때 방법에서 reduce하는 방법만 달라진다. 
1. $ACTION[i, a]:= shift \; j, \; if \; [A\rightarrow \alpha.a\beta]\in I_i\; and \; GOTO(I_i, a) = I_j$ 
2. $ACTION[i, a]:= \; reduce \; A\rightarrow \alpha, if\; [A\rightarrow\alpha., a] \in I_i$
3. $ACTION[i,\$]:= accept\; if \; [S'\rightarrow S.] \in I_i$
4. $GOTO(i, A) := j \; if \; GOTO(I_i, A) = I_j$
5. error for all undefinded entries and initial state is $i \; if \; [S'\rightarrow.S]\in I_i$

### LALR Parsing Table
LALR을 구성하는 방법은 두 가지 방법이 존재한다. 
#### $C_1$
$C_1$을 이용하는 방법은 core가 같은 LR(1) items들을 합치는 것이다. 이렇게 합치는 작업을 수행하게 되면 SLR과 크기가 같은 parsing table이 구성된다. 

일반적으로 이 작업에서는 shift/reduce간의 충돌은 발생하지 않는다. reduce/reduce간의 충돌은 발생할 수 있다. 
#### $C_0$
위에서 언급한대로 $C_1$을 만들고, merge하는 방식은 충돌의 가능성이 존재하고, 시간과 공간을 많이 소요한다. 그러므로 일반적으로 상대적으로 시간과 공간을 적게 소모하며 충돌의 가능성이 적은 $C_0$를 이용하는 방법으로 LALR을 구성한다.

$C_0$를 이용하는 방법은 먼저 $C_0$를 구성하고, reduce item에 대해서만 [[LOOKAHEAD]]를 계산해 이를 이용해 reduce하는 것이다. 이를 위해 먼저 [[LOOKAHEAD#Efficient Computaion of LOOKAHAED Sets]]를 다음과 같이 정의한다.
![[LOOKAHEAD#Efficient Computaion of LOOKAHAED Sets]]

$C_1$을 구성할 때는 [[LOOKAHEAD]]을 계속해서 계산했기 때문에 계산이 쉬웠지만, 이 경우에서는 계산은 다음과 같은 규칙을 통해 이뤄진다.$$LA(p, \; [A\rightarrow\alpha.\beta]) = \bigcup_{q\in PRED(p, \alpha)} \;\bigcup_{[B\rightarrow\alpha_1.A\alpha_2] \in q} FIRST(\alpha_2) \oplus LA(q, \; [B\rightarrow\alpha_1.A\alpha_2]$$여기서 $PRED(p,\alpha)=\{q \; | \; p \in GOTO(q, \alpha)\}$로 정의한다.

위 계산은 결국, 현재 상태에서 $\alpha$만큼 거슬러 올라가서 $A$를 marksymbol로 가지는 상태들에서 $\alpha_2$를 확인한다는 의미이다. 
[[LOOKAHEAD]]를 계산하는 알고리즘은 다음과 같다.
```pseudo code
function LA(p: state; I: time): set of Vt;
{
	assume I = [A->a.b];
	if (A == S') LA = {$};
	else {
		LA = {};
		for q in PRED(p, a) do
			for [B->a.Ac] in q do
				LA = LA + FIRST(c);
				if (e in FIRST(c) && !MAP(q, [B->a.Ac]))
					LA = LA + LA(q, [B->a.Ac])
	}
}
```
