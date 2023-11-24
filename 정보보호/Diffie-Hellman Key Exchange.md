1976에 Diffie와 Hellman에 의해 제안된 최초의 public-key algorithm scheme. 하지만, 실제로 James Ellis(UK CESG)에서 1970년에 고안한 방법이라고 한다. 

Public key 분배에 대한 scheme으로, 임의의 메시지 교환에는 사용할 수 없다. [[Prime Number#Primitive Roots]]을 기초로 해 알고리즘이 만들어 졌다. 
## Algorithm
### Key Setup
1. 모든 유저는 다음 global prameter를 사용한다
	+ 충분히 큰 소수나 다항식 $q$
	+ $\alpha$: [[Prime Number#Primitive Roots]] for $\mod q$
2. 각 유저는 key를 생성한다.
	+ private key를 선택: $X_A<q$
	+ public key를 계산: $Y_A = \alpha^{X_A} \bmod q$
3. public key만 통신을 위해 공유한다. 
### Key Exchange
유저 A와 B가 통신하기 위한 절차는 다음과 같다.
1. A와 B는 prime number $q$와 정수 $\alpha$를 공유한다. 
2. A와 B는 각자의 private key와 public key를 생성한다.
3. 그리고 public key를 서로에게 전달한다. 
4. 전달받은 public key를 통해 A와 B는 각각 shared secret key(session key) $K_{AB}$를 계산한다. $$\begin{align} K_{AB} & = (a)^{X_AX_B} \bmod q \\ &=(Y_A)^{X_B} \bmod q & B\; can\; compute \\ &=(Y_B)^{X_A} \bmod q & A\; can \; compute\end{align}$$

여기서 만들어진 $K_{AB}$는 A와 B의 통신 간에서 [[Symmetric encryption]]처럼 사용된다. 만약 공격자가 $Y_A, Y_B$를 통해 $X_A,X_B$를 예측하기 위해서는 [[Prime Number#Discrete Logarithms(or Indices)]]을 계산해야 한다. $$X_B = dlog_{a,q}(Y_B)$$사용자는 단순히 moduler prime에서 제곱을 계산하면 되지만, 공격자는 어려운 계산인 [[Prime Number#Discrete Logarithms(or Indices)]]을 수행해야 하기 때문에 매우 큰 소수 $q$에 대해서는 공격이 불가능하다. 
### Example
$q=353, \alpha =3$으로 결정한 경우의 A와 B의 키 교환
+ private key 결정: A, B가 각자 $X_A = 97, X_B = 233$로 결정
+ pulbic key 계산
	+ A: $Y_A = 3^{97} \bmod 353 = 40$
	+ B: $Y_B=3^{233} \pmod {353} = 248$
+ 공유 session key 계산
	+ A: $K_{AB} = Y_B^{X_A} \bmod {353} = 248^{97} \bmod {353} = 160$
	+ B: $K_{AB} = Y_A^{X_B} \bmod {353} = 40^{233} \bmod{353} = 160$
## Protocols
키 교환을 위한 방법은 다음과 같은 방법들이 존재한다.
+ 각 유저가 random private/public D-H key를 각 통신마다 생성한다. 
+ 각 유저가 알려진 private/public D-H key를 생성해 directory에 저장한 후 보안 통신을 함

하지만 위 방법들 모두 [[Man-In-the-Middle Attack]]에 취약하다. 키에 대한 인증이 필요하다.
## [[Man-In-the-Middle Attack]]
![[Man-In-the-Middle Attack]]
D-H에서 [[Man-In-the-Middle Attack]]은 다음과 같은 과정으로 이루어진다.
1. 공격자 D가 자신의 private/public key를 생성한다
2. A가 B에게 자신의 public key를 전달한다.
3. 이때 D가 A의 public key를 탈취하고 자신의 public key를 B에게 전달한다. 이때 공유 session key를 계산해 A에게 전달한다. 
4. B는 D의 pulbic key를 전달받게 되고, 이 값을 통해 공유 session key를 계산한다. 
5. B는 A에게 자신의 public key를 전달한다.
6. D가 B의 public key를 탈취하고 자신의 pulic key를 A에게 전달한다. 이때 공유 session key를 계산해 B에게 전달한다. 
7. A는 D의 public key를 전달받게 되고, 이 값을 통해 공유 session key를 계산한다.
8. 이후 D는 A와 B 사이에서 변조, 암호화, 복호화 등 메시지에 대한 모든 행위를 할 수 있게 된다. 
