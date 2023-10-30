특수한 meta symbol을 사용하여 [[프로그래밍 언어]]의 구문을 명시하는 표기법으로, meta symbol은 다음과 같이 추가된다. 
+ nonterminal symbol: <>로 감싸 구별한다
+ $\rightarrow$는 ::=로 표기한다. 치환으로 읽는다. 
+ terminal symbol: ''로 감싸 구별한다.

example: $V_N = \{E, T, F\}, V_T = \{+,*,(,),a\}, P= \{E\rightarrow E+T | T, T \rightarrow T * F | F, F \rightarrow (E)|a\}$
BNF expression: $<E>::=<E>'+'<T>|<T> \; <T>::=<T>'*'<F>|<F> \; <F>::='('<E>')'|'a'$ 