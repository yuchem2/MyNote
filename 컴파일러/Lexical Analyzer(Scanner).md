
흔히 Scanner라고 하며, 한국어로는 어휘 분석기이다. 

소스 프로그램을 읽어 들여 일련의 [[토큰(Token)]]을 생성해,컴파일러 내부에서 효율적이며 다루기 쉬운 정수로 바꾸어 준다. 

즉, 입력으로 [[정규 언어(RL, Regular Language)]]를 받아 [[정규 문법(RG, Regular grammar)]]을 통해 [[토큰(Token)]]의 구조를 파악해 변환한다. [[CFG(Context-free grammar)]]를 이용해서 구현할 수도 있지만 [[정규 문법(RG, Regular grammar)]]에 비해 비효율적이므로 일반적이지 않다. 

[[Compiler]] 구조에서 첫 번째에 위치한 구조이지만, 실제로는 [[Syntax Analyzer(Parser)]]의 subprocedure로 사용된다. 즉, [[Syntax Analyzer(Parser)]]의 요청에 따라 토큰을 반환해 [[Syntax Analyzer(Parser)]]로 전달하는 역할을 수행한다.

여기서 [[토큰(Token)]] 타입을 전달하며 이는 (token number, token value)의 형태를 띈다


## **Design Steps**
---
1) Describe the structure of tokens in [[정규 표현(RE, Regular Expression)]] or, directly design a transition diagram for the tokens.
2) Program a scanner according to the diagram.
3) Moreover, we *verify* the scanner action through [[정규 언어(RL, Regular Language)]] theory


### Identifier Recognition

### Integer number Recognition

### Real number Recognition

### String Constant Recognition

### Comment Recognition


## **Implements**
---
+ Programming: 직접 [[프로그래밍 언어]]를 이용
+ Generating: [[Lexical Analyzer Generator]]를 이용 