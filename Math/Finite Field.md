> A subset of [[Field]], consiting of those fields with a finite number of elements

암호학에서 점점 더 중요하게 생각하는 수학적 정의로, [[AES(Advanced Encryption Standard)]]와 같은 수많은 [[암호화 알고리즘(Cypotographic algorithms)]]과 elliptic curve cyptography이 Finite Field의 특성에 크게 의존한다. 또한 [[MAC(Message Authentication Code)]]인 [[CMAC]]과 [[인증(Authentication)]]된 암호화 방식 GCM도 이러한 특성을 기초하고 있다

## Galois Field
수학자 *Galois*는 [[Finite Field]]를 처음 연구한 사람으로, 이 사람의 이름을 따 [[Finite Field]]는 다음과 같은 방법으로 표기한다
+ $GF(p^n)$
+ $GF(p)$
+ $GF(2^n)$
## GF(p)
> For a given prime $p$, we define the finite field of order $p$, $GF(p)$, as the set $Z_p$ of integers $\{0, 1, ..., p-1\}$ togetehr with the arithmetic operations modulo $p$. 

> We futher observed that any integer in $Z_n$ has a multiplicative inverse iff that integer is relatively prime to $n$. *If $n$ is prime, then all of the nonzero integers in $Z_n$ are relatively prime to $n$,and therefore there exists a multiplicative inverse for all of the nonzero integers in $Z_n$.* Thus, for $Z_p$ we can add the following properties to those listed in:
> 	Multiplicative inverse($\omega^{-1}$): For each $\omega \in Z_p, \omega \neq 0, there \; exists \; a \; z \in Z_p \; such \; that \; \omega \times z \equiv 1 \pmod {p}$

위 특성을 보면 곱셈 역원이 성립하므로 다음과 같은 식을 세울 수 있다. 
$$\begin{align} for \; a \; and \; b \; in \;Z_p \; with \; a \neq 0 \\&& if \; (a\times b) \equiv (a\times c)\pmod{p} && then \qquad b \equiv c \pmod{p} \end{align}$$
### Finding the multiplicative Inverse in GF(p)
> If $a$ and $b$ are relatively prime, then $b$ has a multiplicative inverse modulo $a$. That is, if $gcd(a, b) = 1$, then $b$ has a multiplicative inverse modulo $a$. Thus, for positive integer $b < a$, there exists a $b^{-1} < a$ such that $bb^{-1} \equiv 1 \bmod a$. If $a$ is a prime number and $0 <b <a$, then clearly $a$ and $b$ are relatviely prime and have a greatest common divisor of $1$. 

[[GCD(Greatest Common Divisor)#Euclidean Algorithm]]을 확장시키면 다음과 같다. 
$$ax + by = d = gcd(a, b)$$
$gcd(a, b) = 1$이면 $ax+by = 1$이다. Basic equalities of [[Modular Arithmetic]]을 사용하면 아래와 같다.
$$\begin{matrix}[(ax \bmod a) + (by \bmod a)]\bmod a = 1 \bmod a \\ 0 + (by\bmod a) = 1\end{matrix}$$
위 식이 성립하려면 $y=b^{-1}$이여야 한다. 즉, $gcd(a, b)=1$일 때 $b$는 곱셈 역원을 가진다.

$p$가 prime number인 경우 $Z_p$는 $\phi(p)$와 동일하고, 이 경우에 모든 원소 $r$이 $gcd(r, p)=1$를 만족한다. 그러므로 $GF(p)$는 덧셈, 뺄셈, 곱셈, 나눗셈에 대해서$\pmod p$닫혀있음을 증명할 수 있다. 이러한 경우에 $r$의 역원은 $r \times x \bmod p = 1$을 만족하는 $x$가 되고, $y/r = (y\times r^{-1})\bmod p = (y\times x) \bmod p$라고 할 수 있다. 

![[Pasted image 20231020140717.png | 600]]
<div align="center">Arithmetic Modulo 8 and Modulo 7</div>

## GF($2^n$)
대부분의 디지털 시스템에서는 2진수를 통해 데이터를 처리한다. 그러므로 각 데이터는 bit로 저장되는데 bit에 대한 낭비를 줄이기 위해 특별히 $p=2, GF(2^n)$를 정의한다. 

만약 기존의 GF를 이용해 bit값이 가지는 수를 표현하기 위해선 소수를 이용해야 하는데, $2^n$은 $n=1$이 아닌 경우에는 항상 소수가 아니게 된다. 그러므로 $2^n$보다 낮은 수로 GF를 정의해야하고, 이는 곧 표현할 수 있는 값이 줄어듦을 의미한다. 

또한 이렇게 정의하는 경우 0이 아닌 정수에 대한 분포가 [[Uniform distribution]]하게 정의가 된다. $Z_8$의 경우 4가 가장 많이 등장하게 되는데, $Z_{2^3}$의 경우에는 0이 아닌 가능한 모든 수가 동일한 분포를 가짐을 확인할 수 있다. 그리고, prime이 아닌 8에 대해서도 곱셈 역원을 정의할 수 있게 된다. 

$GF(2^n)$의 경우 가능한 수의 집합은 $Z_{2^n}$로 한정한다. $a, b$를 임의의 $Z_{p^n}$의 원소에 대해 $a={a_na_{n-1}..a_0}_{(2)},$ $b={b_nb_{n-1}..b_0}_{(2)}$ 라고 하면, 연산은 다음과 같이 정의된다.$$\begin{align}addition:& \; a + b = a \oplus b \\ multiplication: & \;a\times b = (a\times b) \oplus r \quad where \; r \; is \; minimum \; prime \; bigger\; than \; 2^n \end{align}$$이를 [[Polynominal Arithmetic#Polynomial with coefficients in $Z_p$]]에서 $p=2$로 하여서 표현하면 간단하게 덧셈, 곱셈을 정의할 수 있다

e.g. $n=3, r=1011_{(2)} = 11$

![[Pasted image 20231020145006.png | 600]]
<div align="center">Arithmetic in GF(2^3)</div>
