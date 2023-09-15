
선택적인 기능이다. [[Complier]] 마다 있을 수 도 있고, 없을 수도 있다. 

비효율적인 코드를 구분해내서 더 효율적인 코드로 변환해주는 기능을 수행한다.

+ 최적화 의미
	+ Major: 실행 시간 향상
	+ Minor: 코드 크기 단축
+ 최적화 기준
	+ 프로그램의 의미를 유지하는 선에서 이루어져야 한다.
	+ 평균적으로 향상되어야 한다.(빨라져야 한다)
	+ 노력에 가치가 있어야 한다. 

최적화 종류
+ Local optimization: 하나의 블록 내에서 local inspection을 통하여 비효율적인 코드들을 구분해 내서 좀 더 효율적인 코드로 바꾸는 방법
	+ Common subexpression (공통 부분식 제거)
	+ Strength reduction (연산 강도 경감)
	+ Constant folding & propagation
	+ Algebraic simplification (대수학적 간소화)
+ Global optimization: 흐름 분석 기술을 이용한다. (블록 간의 정보를 고려)
	+ Common subexpression
	+ Moving loop invariants
	+ Removing unreachable codes

※ Global optimization에서는 Removing unreachable code가 주로 채택되고 나머지는 효율성에 따라 상이하게 채택된다. 