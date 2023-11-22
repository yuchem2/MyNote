
특정 [[고급 프로그래밍 언어]]로 작성된 프로그램을 실행 파일로 변환해주는 컴퓨터 프로그램을 말한다. 

정확히는 컴파일 과정에서 특정 [[고급 프로그래밍 언어]]로 작성된 소스 코드를 목적 파일로 변환해주는 프로그램을 말한다. 아래 그림이 [[Compile Procedure]]이다. 

![[Pasted image 20230915213308.png]]

컴파일러와 동일하게 "번역기"와 같은 기능을 수행하는 프로그램은 다음과 같다. 
![[Cross-Complier]]
![[Interpreter]]
![[Preprocessor]]


## **일반적인 컴파일러 구조**
Front-end 부분과 Back-End 부분으로 나눌 수 있다. 
Front-end 부분은 언어 의존적이고, Back-End 부분은 기계 의존적이다. 

+ Front-end
	+ [[Lexical Analyzer(Scanner)]]
	+ [[Syntax Analyzer(Parser)]]
	+ [[Intermediate Code Generator]]
+ Back-end
	+ [[Code Optimizer]]
	+ [[Target Code Generator]]


여기서 Analysis Phase를 [[Lexical Analyzer(Scanner)]]와 [[Syntax Analyzer(Parser)]]가 담당하는데, 이를 구분해 놓은 이유는 아래와 같다.
+ Modular construction(simple design)
+ compiler efficiency is improved
+ compiler portability is enhanced

추가적인 컴파일러의 주요 기능은 [[Error Recovery(Complier)]] 기능이다. 이는 [[Error Repair]]와 다른 기능이다.

컴파일러에서 가능한 에러는 아래와 같다.
+ Error
	+ Syntax
	+ Semantic
	+ Run-time Error
+ Error Handling
	+ Error detection
	+ [[Error Recovery(Complier)]]
	+ Error Reporting
	+ [[Error Repair]]


컴파일러를 자동으로 생성해주는 [[Compiler Generating Tools]]가 존재한다. 