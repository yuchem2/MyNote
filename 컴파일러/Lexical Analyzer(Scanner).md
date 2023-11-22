## Consepts

흔히 Scanner라고 하며, 한국어로는 어휘 분석기이다. 

소스 프로그램을 읽어 들여 일련의 [[토큰(Token)]]을 생성해,컴파일러 내부에서 효율적이며 다루기 쉬운 정수로 바꾸어 준다. 

즉, 입력으로 [[정규 언어(RL, Regular Language)]]를 받아 [[정규 문법(RG, Regular grammar)]]을 통해 [[토큰(Token)]]의 구조를 파악해 변환한다. [[CFG(Context-free grammar)]]를 이용해서 구현할 수도 있지만 [[정규 문법(RG, Regular grammar)]]에 비해 비효율적이므로 일반적이지 않다. 

[[컴파일러(Compiler)]] 구조에서 첫 번째에 위치한 구조이지만, 실제로는 [[Syntax Analyzer(Parser)]]의 subprocedure로 사용된다. 즉, [[Syntax Analyzer(Parser)]]의 요청에 따라 토큰을 반환해 [[Syntax Analyzer(Parser)]]로 전달하는 역할을 수행한다.

여기서 [[토큰(Token)]] 타입을 전달하며 이는 (token number, token value)의 형태를 띈다
## **Design Steps**
1) Describe the structure of tokens in [[정규 표현(RE, Regular Expression)]] or, directly design a transition diagram for the tokens.
2) Program a scanner according to the diagram.
3) Moreover, we *verify* the scanner action through [[정규 언어(RL, Regular Language)]] theory
### Identifier Recognition
+ $\Sigma = \{ l, d, \_ \}$ 
+ FA
![[Pasted image 20231011180443.png | 500]]
+ RG: $S \rightarrow lA \; | \; \_A \quad A \rightarrow lA \; | \; dA \; | \; \_A \; | \; \epsilon$
+ RE: $S = (l+\_)(l+\_+d)^*$
### Integer number Recognition
+ $\Sigma = \{ d, n, o, h, x, X\}$ $n$은 0을 제외한 한자리 수, $o$는 0-7까지 수, $h$는 0-9와 a-f를 의미 
+ FA
![[Pasted image 20231011181234.png | 500]]
+ RG: $S \rightarrow nA \; | \; 0B \quad A \rightarrow dA \; | \; \epsilon \quad B \rightarrow oC \; | \; xD \; | \; XD \; | \; \epsilon \quad C \rightarrow oC \; | \; \epsilon \quad D \rightarrow hE \quad E \rightarrow hE \; | \; \epsilon$
+ RE: $S = nd^*+0+0o^+ +0(x+X)h^+$
### Real number Recognition
+ $\Sigma = \{ d, ., +, -, e\}$
+ FA
![[Pasted image 20231011185816.png | 500]]
+ RG: $S \rightarrow dA \; | \; .C \quad A \rightarrow dA \; | \; .B \; | \; eD \quad B \rightarrow dB \; | \; eD \; | \; \epsilon \quad C \rightarrow dB \quad D \rightarrow dE \; | \; +F \; | \; -F$ 
	  $E \rightarrow dE \; | \; \epsilon \quad F \rightarrow dE$
+ RE: $S = \; .d^+ + \; .d^+e('+'+-+\epsilon)d^+ + d^+e('+'+-+\epsilon)d^+ + \; d^+.d^*+ d^+.d^*e('+'+-+\epsilon)d^+$
### String Constant Recognition
+ $\Sigma = \{ ^", a, c,  \setminus\}$ $a = char\_set - \{^", \setminus\}, \; and \; c=char\_set$
+ FA
![[Pasted image 20231011192118.png | 500]]
+ RG: $S\rightarrow$ $^"$$A \quad A \rightarrow aA \; | \; \backslash C \; | \; ^"B \quad B \rightarrow \epsilon \quad C \rightarrow cA$
+ RE: $S =$ $^"$$(a+\backslash c)^*$$^"$
### Comment Recognition
※ Comment는 컴파일되는 부분은 아니지만 컴파일에서 제외시키기 위해 인식할 필요가 있음
+ $\Sigma = \{ a, b, *, /\}$ $a = char\_set - \{*\}, \; and \; b=char\_set - \{*, /\}$
+ FA
![[Pasted image 20231011193925.png | 500]]
+ RG: $S \rightarrow /A \quad A \rightarrow *B \quad B \rightarrow aB \; | \; *C \quad C \rightarrow *C \; | \; /D \; | \; bB \quad D \rightarrow \epsilon$
+ RE: $S = /*(a+*^+b)^* *^+/$
## **Implements**
+ Programming: 직접 [[프로그래밍 언어]]를 이용
+ Generating: [[Lexical Analyzer Generator]]를 이용 