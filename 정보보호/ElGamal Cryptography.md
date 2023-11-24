[[Diffie-Hellman Key Exchange]]와 관련이 있는 public key scheme. [[Diffie-Hellman Key Exchange]]과 동일한 방법의 연산을 사용한다. 
## Algorithm
### Key Setup
1. 모든 유저는 다음 global prameter를 사용한다
	+ 충분히 큰 소수나 다항식 $q$
	+ $\alpha$: [[Prime Number#Primitive Roots]] for $\mod q$
2. 각 유저는 키를 생성한다. 
	+ private key를 선택: $1< X_A < q-1$
	+ public key를 계산: $Y_A = \alpha^{X_A} \bmod q$ 
3. public key($\{q, \alpha, Y_A\}$)를 통신을 위해 공유한다. 
### Message Exchange
B가 A에게 메시지를 암호화해 전송한다고 할 때 다음과 같은 과정으로 진행된다. 이때 private key와 public key는 생성되었고, public key는 공개되었다고 가정한다. 
1. 보낼 메시지 $M$를 $0\leq M \leq q-1$의 범위로 표현한다. 이 범위보다 큰 $M$은 block으로 잘라 암호화한다.
2. $1\leq k \leq q-1$인 임의의 정수 $k$를 선택한다. 
3. One time key를 계산한다. $K=Y_A^k \bmod q$ 
4. 메시지 $M$을 정수 쌍($C_1, C_2$)으로 암호화한다. $$C_1 = \alpha^k \bmod q \quad C_2 = KM \bmod q$$
5. 전달 받은 암호문을 A가 복호화한다. 먼저 K를 계산한다. $$K = (C_1)^{X_A} \bmod q$$
6. 메시지를 복호화한다. $$M = C_2K^{-1}\bmod q$$
위 과정에서 보면 복호화에서 $k$가 직접적으로 사용되지 않음을 확인 할 수 있다. 또한 $k$는 각 통신마다 유일한 값으로 결정해야 안전한다. 
### Example
$q=19, \alpha=10$으로 결정한 경우의 A와 B의 메시지 교환
+ A가 자신의 key 계산: $X_A= 5, Y_A = 10^5 \bmod 19 = 3$
+ B가 메시지 $M=17$을 암호화. $k=6, K=3^6 \bmod 19=7$ $$C_1 = 10^6 \bmod 19 = 11 \quad C_2 = 7\times 17 \bmod 19 =5$$
+ A가 전달받은 메시지를 복호화$$K=11^5\bmod 19 = 7 \quad M = 5\times7^{-1}\bmod 19 = 5\times 11 \bmod 19=17$$