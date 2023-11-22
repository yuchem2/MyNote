1977년 MIT에서 *Rivest, Shamir & Adelman*에 의해 개발된 [[Asymmetric encryption]]기술로, 가장 유명하고 널리 사용되는 public-key scheme이다. 

Modulo prime의 [[Finite Field#Galois Field]]에서의 정수 지수화를 기초로 작성되었다. 
이로 인해 정당한 사용자는 연산에 $O((log\; N)^3)$가 소요되고 공격자는 연산에 $O(e^{log\; N \; log \; log \; N})$가 소요된다. 일반적으로 $1024\sim2048\;bits$ 크기의 정수를 사용한다. 
## Algorithm
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
### Exponentiation in [[Modular Arithmetic]]
RSA에서 암호화, 복호화 모두 $\bmod n$에서의 정수 거듭 제곱 연산을 수행한다. RSA에서는 매우 큰 수를 사용하고 있기 때문에 큰 수에 대한 제곱 연산을 계산해야 하는데, 효율적이고 빠른 방법으로 [[Square and multiply Algorithm]]을 사용할 수 있다. 이 기법을 사용한다면 $n$에 대해 $O(log_2\;n)$만 소요된다. 
![[Square and multiply Algorithm]]

### Efficient Opertaion Using the Public Key
Public key를 사용할 때 RSA의 연산의 속도를 늘리기 위해 작은 $e$를 선택해 사용한다. 가장 많이 사용하는 값은 $e=65537(2^{16}+1)$이고, 그 다음으로는 $e=3 \;or\;e=17$ 순으로 사용한다. 이들은 각각 2개의 1 bit 값만 가져 지수화를 수행하는데 필요한 곱셈의 수가 최소화가 된다. 

하지만 $e=3$과 같이 매우 작은 경우 RSA는 간단한 공격에도 취약해진다. [[CRT(Chinese Remainder Theorem)]]과 3명의 사용자에게 message를 보내면서 유추가 가능하다.
e.g. RSA가 3개의 다른 유저에게 동일하게 $e=3$을 사용하고, $n$이 각각 $(n_1, n_2, n_3)$이라고 가정한다. $A$가 3명의 유저들에게 동일한 메시지 $M$를 보내면 3개의 암호문이 만들어진다. $$\begin{align}C_1 &= M^3\bmod n_1\\C_2 &= M^3\bmod n_2\\C_3 &= M^3\bmod n_3\end{align}$$여기서 $n_1, n_2, n_3$는 각각이 서로소 관계를 띌 가능성이 있다. 여기서 [[CRT(Chinese Remainder Theorem)]]를 사용해 $M_3 \bmod (n_1, n_2, n_3)$를 계산한다. RSA 알고리즘에 따르면 $M$은 각 $n_i$보다 작기 때문에 $M^3 < n_1n_2n_3$가 만족한다. 이에 따라 공격자는 $M^3$의 세제곱근만 계산하면 된다. 이 공격방법은 메시지 $M$의 각 인스턴스에 *unique pseudorandom bit string*을 padding하여 대항할 수 있다. 

$e$는 RSA의 알고리즘에 따라 유저에 이해 $e$가 선택되는데 $e$의 값은 $gcd(e, \phi(N))=1$를 만족해야 한다. 그러므로 $e$를 선택한 뒤에 $p, q$를 선택하며 $p, q$가 $e$와 서로소가 아닌 경우 reject하고 새로운 $p, q$를 생성하면 된다.
### Efficient Operation Using the Private Key
Private key를 사용할 때 RSA의 연산의 속도를 늘리기 위해 작은 $d$를 사용할 수 없다. 작은 $d$값은 [[Brute-force attack]]과 다른 형태의 [[Cryptanalysis]]에 취약하기 때문이다. 

그러나 [[CRT(Chinese Remainder Theorem)]]을 통해 계산 속도를 증가시킬 수 있다. $M=C^d\bmod n$을 계산하기 위해 다음과 같은 과정을 수행한다. $$V_p = C^d \bmod p \quad V_q = C^d \bmod q$$[[CRT(Chinese Remainder Theorem)]]에 따라 다음과 같이 정의한다. $$X_p = q \times(q^{-1}\bmod p) \quad X_q = p(p^{-1}\bmod q)$$그러면, [[CRT(Chinese Remainder Theorem)]]에 따라 $$M = (V_pX_p + V_qX_q)\bmod n$$[[Fermat's Theorem]]을 이용해 $V_p, V_q$를 간단하게 계산할 수 있다. $$V_p=C^d\bmod p=C^{d\bmod(p-1)}\bmod p \quad V_q = C^d\bmod q = C^{d\bmod(q-1)}\bmod q$$ 여기서 $d\bmod (p-1)$과 $d\bmod (q-1)$은 미리 계산해 사용할 수 있다. 위 연산의 결과는 $M=C^d\bmod n$을 직접 계산하는 것보다 대략 4배 정도 빠르다. 
### RSA Key Generation
각 유저의 RSA는 무조건 다음을 수행한다. 
+ prime number $p, q$를 랜덤하게 결정한다. 
+ $e, \; d$ 둘 중 하나를 선택하고 하나를 계산한다.
#### Select Random Prime Number
$N=p\times q$이기 때문에 $p, q$를 결정해야 한다. 이때 $N$은 공개되기 때문에 이를 통해 $p, q$가 유추 불가능하게 결정해야 한다. 완벽한 방법으로는 $p, q$를 충분히 큰 집합에서 선택하는 것이다. 반면에 큰 소수를 찾는 과정이 효율적이어야 한다. 

현재로서는 임의의 큰 소수를 찾는 유용한 기술이 존재하지 않는다. 일반적으로 사용되는 방법은 원하는 크기의 홀수를 임의로 선택하여 그 수가 소수일 때까지 연속된 난수를 생성하는 것이다. 

소수를 찾는 여러 가지 검증이 존재하지만 모두 *probabilistic test*(확률적인 검증)이다. 즉, 검증은 단지 주어진 정수가 아마도 소수일 것이라고 판단한다. 확실성이 부족함에도 이 검정들은 확률을 1에 가깝게 만드는 방법으로 실행될 수 있다. [[Prime Number#Miller-Rabin Algorithm]]과 같은 알고리즘을 사용할 수 있다. 

```ad-summary
title: Procedure for picking a prime number

1. [[PRNG(Pseudo-Random Number Generator)]]을 이용해 홀수 정수 $n$을 선택
2. $a<n$인 정수 $a$를 랜덤하게 결정
3. Probabilistic primality test(e.g. [[Prime Number#Miller-Rabin Algorithm]])을 $a$를 파라미터로 해 수행. $n$이 test에 실패하면 1로 돌아가 다시 수행
4. $n$이 충분한 수의 test를 통과하면 $n$을 accept. 아직 충분하지 않으면 2로 돌아가 다시 수행
```

위 알고리즘은 $p, q$를 결정하는데 얼마나 많은 시간이 소요될 지 결정할 수 없지만 새로운 $PU, PR$을 만들 때만 필요하다. 평균적으로 소요되는 시간은 [[Prime Number#Distribution of Primes]]에 따라 $ln(N)\div2 = ln(2^200)\div2 = 70$번의 시도가 필요하다.
#### Calculation of Multiplicative Inverse
$p, q$를 결정한 뒤 key generation에서는 $e, d$를 결정해야 한다.
+ $gcd(e, \phi(N))=1$를 만족하는 $e$를 결정한 뒤 $d \equiv e^{-1}\pmod{\phi(n)}$ 
+ $gcd(d, \phi(N))=1$를 만족하는 $d$를 결정한 뒤 $e \equiv d^{-1}\pmod{\phi(n)}$ 

일반적으로 첫 번째 방법이 많이 사용되며 곱셈 역원을 구할 때 [[GCD(Greatest Common Divisor)#Euclidean Algorithm]]을 이용할 수 있다. 아래 예시에서 $p=\phi(n)$으로 적용하면 된다. 
![[Finite Field#Finding the multiplicative Inverse in GF(p)]]
## Security
RSA를 공격하는 접근은 다음과 같은 방법들이 존재한다.
+ [[Brute-force attack]]: 현재로써는 실현 불가능한 방법
+ Mathmeatical attacks: factoring the product of two primes
+ [[Timing Attacks]] on running decryption
+ [[Cryptanalysis]]-chosen ciphertext attacks: exploits properties of the RSA algorithm
### Factoring Problem
RSA를 수학적으로 공격하는 방법 중 하나. 다음과 같은 방법들이 존재한다.  
+ Factor $N$을 소인수분해를 통해 두 개의 소수로 나눈 뒤 $\phi(N)=(p-1)(q-1)$을 계산할 수 있으며, 이를 통해 $d\equiv e^{-1} \pmod{\phi(n)}$를 찾는다.
+ $p, q$를 결정하지 않고, 직접적으로 $\phi(n)$를 결정한 뒤 $d\equiv e^{-1} \pmod{\phi(n)}$를 찾는다.
+ $\phi(n)$을 결정하지 않고, 직접적으로 $d$를 결정한다.

이 세 방법에 대한 공격 방법들은 수년 간 느리게 향상되고 있었지만, 효율적인 알고리즘(e.g. [[QS(Quadratic Sieve]], [[GNFS(Generalized Number Field Sieve)]], [[LS(Lattice Sieve)]])가 등장하면서 빠르게 발전하고 있다. 하지만 여전히 $1024\sim2048\;bit$ RSA는 안전하다.

| Number of Decimal Digits | Approximate Number of Bits | Date Achieved | MIPS-years |                Algorithm                 |
|:------------------------:|:--------------------------:|:-------------:|:----------:|:----------------------------------------:|
|           100            |            332             |  April 1991   |     7      |          [[QS(Quadratic Sieve]]          |
|           110            |            365             |  April 1992   |     75     |          [[QS(Quadratic Sieve]]          |
|           120            |            398             |   June 1993   |    830     |          [[QS(Quadratic Sieve]]          |
|           129            |            428             |  April 1994   |    5000    |          [[QS(Quadratic Sieve]]          |
|           130            |            431             |  April 1996   |    1000    | [[GNFS(Generalized Number Field Sieve)]] |
|           140            |            465             | February 1999 |    2000    | [[GNFS(Generalized Number Field Sieve)]] |
|           155            |            512             |  August 1999  |    8000    | [[GNFS(Generalized Number Field Sieve)]] |
|           160            |            530             |  April 2003   |     -      |          [[LS(Lattice Sieve)]]           |
|           174            |            576             | December 2003 |     -      |          [[LS(Lattice Sieve)]]           |
|           200            |            663             |   May 2005    |     -      | [[LS(Lattice Sieve)]]                                         |

![[Pasted image 20231121112243.png|600]]

<div align="center">Progess in Factoring & Shor's Factorizing Algorithm</div>
### [[Timing Attacks]]
1990년대 중반에 RSA를 공격할 [[Timing Attacks]]이 개발되었다. 큰 값과 작은 값을 곱할 때 연산이 수행되는 시간이 다른 것을 악용한 공격이다. 피연산자의 크기에 따라 연산 시간이 결정되기 때문이다. 이를 방지하기 위해 다음과 같은 방법들이 존재한다. 
+ Use constant exponentiation time: 모든 exponentiation이 같은 시간이 소요되게 성능의 정도를 고정시켜 사용한다. 
+ Random delay: 랜덤한 딜레이를 연산에 포함
+ Blinding: exponentiation을 수행하기 전에 랜덤한 수로 암호문을 곱한다. 
### CCA(Chosen Ciphertext Attacks)
기본적인 RSA는 CCA에 취약하다. 공격자는 평문을 선택하여 평문을 선택해 대상의 public key로 암호화한 후 private key로 복호화함으로써 평문을 되찾을 수 있다. 이 방법을 통해 공격자는 새로운 정보를 얻을 수 없지만 대신 RSA의 특징을 통해 private key로 처리된 암호문에서 필요한 정보를 얻을 수 있는 데이터 블록을 선택할 수 있다. 이를 방지하기 위해 다음과 같은 방법들이 존재한다. 
+ 평문에 random한 pad를 추가한다.
+ Optimal Asymmetric Encrpytion padding(OAEP)를 사용한다.

