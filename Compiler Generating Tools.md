
Compiler-Compiler라고도 한다.

[[프로그래밍 언어]]와 기계가 발달할 수록 많은 [[Compiler]]가 필요하기 때문에 이를 자동적으로 생성해주는 도구의 필요성이 생겼다. 

이상적인 모델은 Language description과 machine description를 입력 받아 [[Compiler]]를 출력하는  형태이다. 그러나 Language description은 문법론을 이용하는 데 반해 machine description은 정형화가 이루어져 있지 않은 상태이므로 현실적으로 구현이 어렵다.

Machine architecuture와 [[프로그래밍 언어]]의 발전에 따라 자동 컴파일러 생성기가 연구되고 있다. 

+ [[Lexial Analyzer Generator]]
+ [[Parser Generating System]]
+ [[Automatic Code Generation]]
+ Compiler-Compiler System
	+ PQCC(Production-Quality Compiler Complier Systme): W.A Wulf(Carnegie-Mellon Univ)
	   입력으로 language description과 target machine description을 받아 PQC와 table이 출력된다.중간 언어로 [[Tree]] 구조인 TCOL(Tree Common Oriented Language)를 사용한다. Pattern Matching Code Generation에 의해 코드를 생성한다.
	+ ACK(Amsterdam Copiler Kit): Andrew S. tanenbaum (Vrije Univ)
	  Compiler의 Back-End 자동화 도구. EM(Encoding Machine)이라는 Abstract Machine Code를 중간 언어로 사용. Portable Compiler를 만들기에 편리.