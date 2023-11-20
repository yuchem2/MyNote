1977년 MIT에서 *Rivest, Shamir & Adelman*에 의해 개발된 [[Asymmetric encryption]] 기술로, 가장 유명하고 널리 사용되는 public-key scheme이다. 

Modulo prime의 [[Finite Field#Galois Field]]에서의 정수 지수화를 기초로 작성되었다. 
이로 인해 정당한 사용자는 연산에 $O((log\; N)^3)$가 소요되고 공격자는 연산에 $O(e^{log\; N \; log \; log \; N})$가 소요된다. 일반적으로 $1024\sim2048\;bits$ 크기의 정수를 사용한다. 
## Algorithm
---
기본적으로 평문은 블럭으로 나누어져 암호화가 진행된다. 블럭 크기 $i$는 $log_2 (n) + 1$보다 작거나 같고, $2^i < n \leq 2^{i+1}$을 만족한다. 암호화와 복호화는 다음과 같은 형식으로 수행된다. $$\begin{align} C & = M^e \bmod n \\ M &=C^d \bmod n = (M^e)^d \bmod n = M^{ed} \bmod n\end{align}$$
여기서 GF(n)에서 곱셈 역원 관계를 만족하는 $e, d$ 즉,  $d \equiv e^{-1} \bmod n$를 만족하는 키를 찾기 위해 [[Euler Totient Function#Euler's Theorem]]를 이용한다. 
![[Euler Totient Function#Euler's Theorem]]
### Key Setup
각 사용자는 public/private key 쌍을 다음과 같은 방법으로 생성한다. 
1. large prime number인 $p,\; q$를 결정한다. 
2. $N = p \times q$ 라고 하면 [[Euler Totient Function]]에 의해 $\phi(N) = (p-1)(q-1)$이다. 
3. 암호화 키 $e$를 다음과 같은 범위에서 결정한다. $$1<e<\phi(N), \quad gcd(e, \phi(N)) = 1$$
4. 다음과 같은 수식을 만족하는 복호화 키 $d$를 결정한다. $$e\times d = 1\bmod \phi(N) \; and \; 0 \leq d < \phi(N)$$ 즉, $d$는 $GF(\phi(N))$에서 $e$의 곱셈 역원($d \equiv e^{-1} \bmod \phi(N)$)
5. public key $KU = \{e, N\}\quad$ private key $KR=\{d, p, q\}$ 
### USE
평문을 M, 암호문을 C라고 했을 때 암호화 복호화는 다음과 같은 방법으로 이루어진다. 이때 평문 M의 길이는 암호화에 사용되는 $N$보다 작아야 한다. 
+ 암호화
	1. 수신자의 public key $KU = \{e, N\}$를 얻는다.
	2. $C = M^e \bmod N, \; where \; 0 \leq M < N$
+ 복호화
	1. 수신자는 자신의 private key $KR =\{d, p, q\}$를 사용한다.
	2. $M = C^d\bmod N$
### Example
+ Key Select
	1. select primes: $p=17, q=11$
	2. $N = 17\times 11 = 187$
	3. $\phi(N) = (p-1)(q-1) = 16 \times 10 = 160$
	4. select $e$: $gcd(e, 160) = 1 \rightarrow$ choose $e=7$
	5. determine $d: d \times e = 1\bmod 160 \; and \; d < 160 \rightarrow d = 23,\;23\times 7 = 161 = 1\times 160 +1$  
	6. publish public key $KU=\{7, 187\}$
	7. keep secret private key $KR = \{23, 17, 11\}$
+ Encryption & decryption
	1. $M = 88$
	2. $C = 88^7 \bmod 187 = 11$
	3. $M = 11^{23} \bmod 187 = 88$
### RSA processing of Multiple Blocks
![[Pasted image 20231120143351.png | 600]]

## Computational Aspects
---
### Exponentiation in [[Modular Arithmetic]]
RSA에서 암호화, 복호화 모두 $\bmod n$에서의 정수 거듭 제곱 연산을 수행한다. RSA에서는 매우 큰 수를 사용하고 있기 때문에 큰 수에 대한 제곱 연산을 계산해야 하는데, 효율적이고 빠른 방법으로 [[Square and multiply Algorithm]]을 사용할 수 있다. 
![[Square and multiply Algorithm]]
