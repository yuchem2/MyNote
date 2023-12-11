[[Random Numbers]]를 생성하는 [[Deterministic Algorithm]] 기술로, 완벽한 [[Random Numbers]]를 생성하는 것이 아니라 많은 수의 테스트를 통과할 정도의 기술을 말한다

완벽하게 [[Random Numbers]]를 생성하는 기술은 TRNG(True Random Number Generator)혹은 RNG라고 하며 이는 현재로서는 실현 불가능한 기술이다

![[Pasted image 20231022160236.png | 600]]
<div align="center">Random and Pseduorandom Number Generators</div>

## Requirements
+ Randomness: uniformity, scalability, consistency(균일성, 확장성, 일관성)
+ Unpredictability: forward & backword unpredictability, 수 많은 테스트 통과
+ Characteristics of the seed: 일반적으로 특정 seed값을 기준으로 [[Random Numbers]]가 생성되는데, 이 seed를 잘 유지하는 것이 중요하다

## Linear Congruential Generator
[[Random Numbers]]를 일반적인 점화식 형태로 정의해 사용한다 $$X_{n+1} = (aX_n+c) \bmod m$$여기서 $m$은 [[Random Numbers]]의 범위의 상한 값이다. 즉 사용자가 조정할 수 있는 값은 $X_0, a, c$이다. 이 값을 조정해 연쇄적으로 [[Random Numbers]]를 생성한다.

하지만 이 경우 값을 잘 조정한다면 마치 실제로 랜덤한 것처럼 보일 수 있지만, 같은 값이 등장할 확률은 상당히 높다. 또한 공격자가 해당 값을 대입하며 수식을 재구성할 가능성도 존재한다.

## Blum Blum Shub Generator
공개 키 알고리즘에 기초한 [[Random Numbers]]를 생성하는 기술이다. 즉, [[Asymmetric Encryption]]에서 기초한 방법이다. [[Random Numbers]]은 다음과 같은 과정을 통해 생성된다. 
1. 먼저 두 개의 큰 [[Prime Number]] $p, q$를 선택한다. 이때 두 수는 4로 나눴을 때 나머지가 3인 수로 한정한다. $$p\equiv q\equiv 3\pmod 4$$
2. 그리고 $n=p\times q$로 정의한 후 $n$과 relatively prime인 임의의 $s$를 선택한다. 그 후 다음과 같은 과정으로 [[Random Numbers]]를 생성한다.$$\begin{align}X_0 & = s^2 \bmod n \\ for\; i & = 1 \; to \; \infty \\ X_i & = X_{i-1}^2 \bmod n \\ B_i & = X_i \bmod 2\end{align}$$
3. 이렇게 생성된 $B_i$는 $bit$ 단위의 각 자리수를 의미하고, 이를 통해 [[Random Numbers]]를 생성한다

이 알고리즘에서 $n$을 만드는 것은 매우 쉽지만, 공격자가 $n$을 예측하기에는 매우 어렵다는 점에서 예측이 매우 어렵다. 매우 큰 수가 사용되기 때문에 느린 것이 단점이지만, 보안성은 매우 높다. 

암호화로 사용되기에는 매우 느리지만 키를 생성하는 데는 매우 효과적인 알고리즘이다

## Using [[Block Cipher]]
[[Block Cipher#Modes of Operation]]를 통해 PRNG를 구현할 수 있다. 이 방법은 주로 master key로부터 session key를 생성할 때 사용된다. [[CTR(Counter)]] 혹은 [[OFB(Output FeedBack)]]를 이용해 주로 생성된다

![[Pasted image 20231022162530.png | 600]]
<div align="center">PRNG Mechanisms Based on Block Ciphers</div>

### [[CTR(Counter)]]
$$X_i = E(K, V_i)$$
### [[OFB(Output FeedBack)]]
$$X_i = E(K, X_{i-1})$$

### ANSI X9.17 PRNG
ANSI에서 공개한 [[Triple DES]]를 이용해 [[Random Numbers]]를 생성하는 PRNG

![[Pasted image 20231022162822.png | 600]]
<div align="center">PRNG using Triple DES</div>

## Natural Random Noise
현실세계에서의 자연적인 랜덤성이 가장 좋은 근원이라고 생각해 이를 이용해 PRNG를 구현하고자 하였다. 이를 위해 규칙적이지만 랜덤한 사건들을 살펴보기 시작하였다. 이를 통해 특정 H/W를 이용하면 이러한 값을 추출할 수 있음을 확인하였다.

일반적으로 특별한 H/W가 필요한데, 다음과 같은 예시들이 존재한다. 
+ Radiation counters
+ Radio noise
+ Audio noise
+ Thermal noise in diodes
+ Leaky capacitors
+ Mercury discharge tubes
+ etc...

### Intel CPU Chip with DRNG

![[Pasted image 20231022163525.png | 600]]
<div align="center">Intel Processor Chip with RNG</div>

![[Pasted image 20231022163608.png | 600]]
<div align="center">Intel DRNG Logical Structure</div>

## Using [[Asymmetric encryption]]
[[Asymmetric Encryption]] 알고리즘은 대략적으로 랜덤인 결과를 출력한다는 기대를 가지고 있다. 그러므로, [[PRNG(Pseudo-Random Number Generator)]]를 만드는데, 사용될 수 있다. 하지만 [[Symmetric encryption]]을 사용하는 것에 비해 느린 단점을 가진다. 이로 인해 일반적으로 짧은 pseudorandom bit sequence를 생성하는데 사용된다. (e.g. key)
### [[RSA]]
Michali-Schonorr PRNG는 [[RSA]]를 사용해 개발되었다. ANSI X9.82와 ISO 18031 표준으로 등록되어 있다. 

![[Pasted image 20231129160702.png | 600]]
### [[ECC(Elliptic Curve Cryptography)]]
[[ECC(Elliptic Curve Cryptography)]]를 이용한 PRNG는 NIST SP 800-9, ANSI X9.82와 ISO 18031 표준으로 등록되어 있다. 보안성과 비효율성 문제에 대한 논쟁이 존재한다.
```pseudo code
p, q are two points on EC
for i = 1 to k 
	set Si = x(Si-1 * p)
	set Ri = lsb240(x(Si * q))
return R1, ..., Rk
```

## Using [[Hash Function]]
Hash PRNG는 SP800-90과 ISO 18031에서 사용된다. Seed 값 $V$를 가지고, 이 $V$ 값에 반복적으로 1을 더하며 사용한다. 결과로 $n$ bit의 hash value가 나오고, 이 값이 random value로 상요된다. 

![[Pasted image 20231207103832.png | 600]]
## Using [[MAC(Message Authentication Code)]]
MAC PRNGs는 SP800-90과 IEEE 802. 11i, TLS에서 사용된다. 초기 $V$ 값을 사용하고, 이후 $V$ 값은 HMAC 연산의 결과가 사용된다. HMAC 기법을 사용하기 때문에 $K$ 값이 필요로 한다. 
![[Pasted image 20231207104214.png | 600]]
