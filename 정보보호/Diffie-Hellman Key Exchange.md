1976에 Diffie와 Hellman에 의해 제안된 최초의 public-key algorithm scheme. 하지만, 실제로 James Ellis(UK CESG)에서 1970년에 고안한 방법이라고 한다. 

Public key 분배에 대한 scheme으로, 임의의 메시지 교환에는 사용할 수 없다. [[Prime Number#Primitive Roots]]을 기초로 해 알고리즘이 만들어 졌다. 
## Algorithm
### Setup
1. 모든 유저는 다음 global prameter를 사용한다
	+ 충분히 큰 소수나 다항식 $q$
	+ $\alpha$: [[Prime Number#Primitive Roots]] for $\mod q$
2. 각 유저는 key를 생성한다.
	+ private key를 선택: $x_A<q$
	+ public key를 계산: $y_A = \alpha^{x_A} \bmod q$
3. public key만 통신을 위해 공유한다. 