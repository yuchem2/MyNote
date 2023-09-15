
특정 [[고급 프로그래밍 언어]]로 작성된 프로그램을 실행 파일로 변환해주는 컴퓨터 프로그램을 말한다. 

정확히는 컴파일 과정에서 특정 [[고급 프로그래밍 언어]]로 작성된 소스 코드를 목적 파일로 변환해주는 프로그램을 말한다. 아래 그림이 컴파일 과정이다. 

![[Pasted image 20230915213308.png]]

컴파일러와 동일하게 "번역기"와 같은 기능을 수행하는 프로그램은 다음과 같다. 
+ [[Cross-Complier]]
+ [[Interpreter]]
+ [[Preprocessor]]


## 일반적인 컴파일러 구조

Front-end 부분과 Back-End 부분으로 나눌 수 있다. 
Front-end 부분은 언어 의존적이고, Back-End 부분은 기계 의존적이다. 

+ Front-end
	+ [[Lexical Analyzer]](Scanner)
	+ [[Syntax Analyzer]](Parser)
	+ [[Intermediate Code Generator]]
+ Back-end
	+ [[Code Optimizer]]
	+ [[Target Code Generator]]

