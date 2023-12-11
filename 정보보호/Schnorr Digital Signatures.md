[[Finite Field#Galois Field]]를 기반으로 하는 [[Digital Signature]]. 

메시지 의존적인 계산을 최소화하는 것을 목표로 설계되었다. 이를 위해 prime number p를 이용해 modulus 연산을 수행한다. 이때 $p-1$은 적절한 크기의 prime factor $q$를 가진다. 일반적인 $p$는 1024 bit 수, $q$는 160 bit 수를 사용한다. 

적절한 prime number $p, q$를 선택하고, $\alpha^q=1\bmod p$를 만족하는 $\alpha$를 선택한다. $p, q, \alpha$를 gobal parameter로 사용한 상태로 각 유저는 다음과 같이 키를 생성한다.
+ Select a private key(secret key): $1<S_A<q$
+ Compute thier public key: $V_A = \alpha^{-S_A} \bmod q$

생성된 키를 통해 A가 B에게 서명을 하고, B가 A의 서명을 검증하는 과정은 다음과 같다. 
1. A가 메시지 M을 다음과 같이 서명하게 된다.
	1. 임의의 정수 $k$ ($0 < k < q$)를 선택하고 $x$를 계산한다. $x=\alpha^k \bmod p$
	2. 메시지와 $x$를 연결한 후에 hash value를 계산한다. $e=H(M\;||\;x)$
	3. $y$를 계산한다. $y=(k+S_Ae)\bmod q$
	4. 서명 $(e, y)$를 구성한다.
2. B가 받은 서명을 다음과 같은 과정으로 검증한다.
	1. $x'=\alpha^yV_A^e\bmod p$
	2. $e = H(M\;||\;x')$를 계산함으로써 검증한다. 
