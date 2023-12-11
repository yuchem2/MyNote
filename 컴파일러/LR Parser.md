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

### Deterministic Parsing of Ambiguous Grammars
모든 모호한 문법은 항상 parsing table을 구성할 때 충돌이 발생한다. shift-ruduce, reduce-reduce간에 발생할 수 있다. 하지만 때때로 특정 언어에 대해서는 유용한 경우가 존재한다.
위에서 말한 두 종류의 충돌은 우선순위와 결합 순서를 통해서 충돌을 없앨 수 있다. 
+ 생성 규칙에 있는 심벌의 우선순위가 shift 심벌의 우선순위보다 높으면 reduce, 그렇지 않으면 shift
+ 생성 규칙에 있는 심벌과 shift 심벌의 우선 순위가 같은 경우에 좌결합이면 reduce, 우결합이면 shift
### Compaction of LR Parsing Tables
parsing table의 크기는 $|states|\times|V|$인데, 이는 문법의 크기가 커질 수록 매우 증가하게 된다. 또한, 앞선 예시들을 살펴보면, table은 일반적으로 빈 공간이 더 많이 존재한다. 이를 통해 table의 크기를 줄이는 몇 가지 방법이 존재한다.
+ action이 동일한 경우에 하나의 표로 관리하고, 상태가 pointer로써 그 action 테이블을 가리키게 만듦
+ 각 상태에서 유효한 terminal symbol에 대한 정보만 유지해 가지고 있는다. 
+ goto table에서 모든 정보만 가지고 있는 것이 아니라, state-nonterminal 쌍이 유효한 경우에만 값을 저장해 가지고 있음. 