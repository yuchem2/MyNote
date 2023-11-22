>A integer $p>1$ is *prime number* iff only divisors are $\pm1$ and $\pm p$. All numbers other than $\pm1$ and the prime numbers are *composite numbers*.

위의 정의에 따라 1보다 큰  임의의 정수 $a$는 다음과 같이 표현할 수 있다. $$a=p_1^{a_1}\times p_1^{a_1}\times p_2^{a_2}\times...\times p_t^{a_t} \quad where \; p_1 < p_2 < ... <p_t \; are \; prime \; numbers\; and \; a_i \; is \; a \; positive \; integer$$위 식을 모든 prime number의 집합 $P$를 통해 표기하면 아래와 같다.$$a= \prod_{p\in P} p^{a_p} \quad where \; each \; q_p \geq 0$$위 식을 통하면 두 수에 대한 곱셈을 다음과 같이 표기 할 수 있다. $$\begin{align}a =\prod_{p\in P} p^{a_p} && b = \prod_{p\in P} p^{b_p} \qquad k=ab \\ k = \prod_{p\in P} p^{k_p} && where \; k_p = a_p+b_p  \; for \; all \; p \in P &\end{align}$$위 $a, b, k$에 대해 다음과 같은 수식도 성립하게 된다. $$if \; a|b, \; then \; a_p \leq b_p \; for \; all \; p$$$$if \; k = gcd(a, b), \; then \; k_p = min(a_p, b_p) \; for\; all\; p$$

![[Fermat's Theorem]]

![[Euler Totient Function]]

## Testing for primality
많은 [[암호화 알고리즘(Cypotographic algorithms)]]에서 하나 혹은 더 많은 크기가 큰 prime number를 랜덤으로 선택하는 것이 필요할 때가 종종 존재한다. 

### Traditionally
trial divison을 이용해 큰 prime number를 찾아냈다. 모든 수에 대해 나누기를 수행해 $Z_p \equiv \phi(p)$인 $p$를 찾았다. 하지만 이러한 것은 매우 많은 시간이 걸렸고, 오직 값이 작은 수에 대해서만 가능했다. 이러한 이유로 통계적인 방법을 사용해 prime number를 찾게 되었다
### Miller-Rabin Algorithm
[[Fermat's Theorem]]를 기초로 한 방법으로 다음과 같은 알고리즘으로 수행된다. 
1. Any positive odd integer $n\geq 3$ can be expressed as $$n-1 = 2^kq \qquad with \; k > 0,\; q \; is \; odd$$
2. Select a random number $a \; where\; 1<a<n-1$
3. $if \; a^q \bmod \; n=1$, then return "inconclusive"
4. $for \; i=0 \; to \; k-1\; do \;that \quad if \; a^{2^iq} \bmod n = n-1$, then return "inconclusive"
5. return "composite"

결과가 composite가 나온다면 $n$은 prime이 아니다. 그 외의 결과의 경우 prime이거나 pseudo-prime이다.  다른 값인 $a$를 $t$번 사용한다고 하면 a가 모두 pseudo-prime일 확률은 최대 $(\frac{1}{4})^t$이다. 그러므로 충분히 큰 값 $t$을 시도한다고 했을 때 항상 inconclusive 결과를 내놓는다면 $n$은 prime이라고 할 수 있다. 

### [[CRT(Chinese Remainder Theorem)]]
![[CRT(Chinese Remainder Theorem)]]
## Distribution of Primes
소수 검정을 사용하여 prime이 발견되기 전에 기각될 가능성이 있는 수가 몇 개인지에 대해 주목할 필요가 있다. 소수 정리로 알려진 수론의 결과는 $n$ 근처의 prime들이 평균적으로 $ln(n)$개의 정수만큼 이격되어 있음을 말하고 있다. 그러므로 평균적으로 소수 검정에는 $ln(n)$ 개의 정수순서에 대해 테스트를 해야 한다. 모든 짝수는 prime이 아니기 때문에 정확한 수치는 $0.5ln(n)$이다. 

e.g. 만약 $n=2^{200}$ 규모에 대한 prime을 찾기 위해서는 약$0.5 ln(2^{200}) = 69$번의 시행이 필요하다. 하지만 이 수치는 단순히 평균에 불과한 수치이다. 

## Primitive Roots
> A primitive root modulo a prime $p$ is an integer $r$ in $Zp$ such that every nonzero element of $Zp$ is a power of $r$

$Z_p$의 어떤 원소 $r$에 대해서 $r^{\phi(p)}\pmod {p}$의 결과가 $Z_p$의 모든 원소와 매핑되는 $r$은 *primitive roots*라고 한다. 

e.g. $p=19$, primitive roots $= 2, 3, 10, 13, 14, 15$

![[Pasted image 20231020132305.png | 600]]
<div align="center">Power of Integers Modulo 19</div>


## Discrete Logarithms(or Indices)
> We know that any intger $b$ satisfies $b \equiv r\pmod{p} \; for\; some \; r, \;where\; 0\leq r \leq (p-1)$ by the definition of [[Modular Arithmetic]]. It follows that for any integer $b$ and a primitive root $a$ of prime number $p$, we can find a unique expoent $i$ such that $$b\equiv a^i \pmod p \quad where \; 0 \leq i\leq (p-1)$$This exponent $i$ is referred to as the discrete logarithm of the number $b$ for the base $a\pmod p$. We denote this value as $dlog_{a, p}(b)$
> 	Note the following:$$\begin{matrix} dlog_{a, p}(1) = 0 \quad becuase \; a^0 \bmod p = 1 \bmod p = 1 \\\\ dlog_{a, p}(a) = 1 \quad becuase \; a^1 \bmod p = a\end{matrix}$$

![[Pasted image 20231020133420.png | 600]]
<div align="center">Tables of Discrete Logarithms, Modulo 19</div>