> A subset of [[Field]], consiting of those fields with a finite number of elements

암호학에서 점점 더 중요하게 생각하는 수학적 정의로, [[AES(Advanced Encryption Standard)]]와 같은 수많은 [[암호화 알고리즘(Cypotographic algorithms)]]과 elliptic curve cyptography이 Finite Field의 특성에 크게 의존한다. 또한 [[MAC(Message Authentication Code)]]인 [[CMAC]]과 [[인증(Authentication)]]된 암호화 방식 GCM도 이러한 특성을 기초하고 있다

## Galois Field
---
수학자 *Galois*는 [[Finite Field]]를 처음 연구한 사람으로, 이 사람의 이름을 따 [[Finite Field]]는 다음과 같은 방법으로 표기한다
+ $GF(p^n)$
+ $GF(p)$
+ $GF(2^n)$
## $GF(p)$
---
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
