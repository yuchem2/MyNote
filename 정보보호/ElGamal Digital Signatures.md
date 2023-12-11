ElGamal에 의해서 제시된 [[Digital Signature]] 방법으로, [[Diffie-Hellman Key Exchange]]과 관련성이 있다. 그러므로 [[Finite Field#Galois Field]]에서의 지수연산을 사용하고 있다.

이 방법은 private key를 통한 암호화를 통해 서명을 제공하고, public key를 통한 복호화로 서명의 검증을 제공하고 있다. 

각 유저는 다음과 같이 자신의 키를 생성한다.
+ Select a private key(secret key): $1<X_A<q-1$
+ Compute thier public key: $Y_A = \alpha^{X_A} \bmod q$

생성된 키를 통해 A가 B에게 서명을 하고, B가 A의 서명을 검증하는 과정은 다음과 같다. 
1. A가 메시지 M을 다음과 같이 서명하게 된다.
	1. 먼저 hash value를 계산한다. $m=H(M), \; 0\leq m\leq q-1$
	2. 임의의 정수 $k$를 선택한다. $1\leq k \leq q-1, \; gcd(k, q-1)=1$
	3. 임시 키 $S_1$을 계산한다. $S_1 = \alpha^k \bmod q$
	4. $k$의 곱셈 역원 $k^{-1}$을 구한다. $k^{-1} = k \bmod (q-1)$
	5. 임시 키 $S_2$을 계산한다. $S_2 = k^{-1} (m-X_AS_1) \bmod (q-1)$
	6. 서명 ($S_1, S_2$)를 구성한다. 
2. B가 받은 서명을 다음과 같은 과정으로 검증한다. 
	1. $V_1=\alpha^m \bmod q$
	2. $V_2 = Y_A^{S_1}S_1^{S_2} \bmod q$
	3. $V_1 = V_2$이면 서명이 유효하다고 판단한다. 
