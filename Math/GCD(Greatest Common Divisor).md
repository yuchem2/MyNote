>The greatest common divisor of $a$ and $b$ is the largest integer that divides both $a$ and $b$,  denoted by $gcd(a, b)$. We also define $gcd(0, 0) = 0$


## Equivalent definition 
$$gcd(a, b) = max[k, \; such \; that \; k|a \; and k|b]$$
GCD는 말 그대로 "최대"이기 때문에 위 식으로 말할 수 있고, 이에 따라 다음과 과 같은 관계들이 성립한다.$$\begin{matrix}gcd(a,b)=gcd(|a|, |b|) \\ gcd(a, 0) = |a|\end{matrix}$$
## Relatively Prime
> Two integers $a$ and $b$ are *relatively prime* iff their only common positive intger factor is $1$. This is euqivalent to saying that $a$ and $b$ are *relatively prime* if $gcd(a,b)=1$


## Euclidean Algorithm
*Euclidean algorithm*이라고 불리는 이 알고리즘은 두 정수의 GCD를 찾는 알고리즘이다.
1. $d = gcd(a, b) = gcd(|a|, |b|)$
2. $b$로 $a$를 나누면서 [[Divisibility#Divison Algorithm]]를 적용하면 다음과 같다$$a=q_1b+r_1\qquad \qquad 0 \leq r_1 < b$$
3. 먼저 $r_1=0$인 경우를 고려한다. 그러면 $b$는 $a$를 나눌 수 있고, 이는 $b$와 $a$를 나누는 가장 큰 수는 $b$라는 의미이다. 즉, $d=gcd(a, b) = b$
4. $r_1 \neq0$인 경우에는 $d|r_1$이라고 할 수 있다. 이는 [[Divisibility#Basic Properties]]에 따라 유도된 식이다. $$d|a \; and \; d|b \rightarrow d|(a-q_1b) \equiv d|r_1 $$
5.  $d|b$와 $d|r_1$인 것이 확인되었다. 임의의 정수 $c$가 $b$와 $r_1$을 나눌 수 있다고 하면, $c|(q_1b+r_1)=a$가 된다. 그러면 $c$는 $a$와 $b$를 둘다 나눌 수 있고, $c$는 $d$보다 작거나 같을 것이다. 그러므로 $d = gcd(b, r_1)$이다. 
6. 그러므로 $gcd(a, b) = gcd(b, r_1)$이라고 쓸 수 있고, $a = b, \; b = r_1$라고 하고 다시 한번 이에 대해 알고리즘을 적용한다. $r_1=0$이 되는 경우 즉, $gcd(a, b) = b$가 될 때까지 반복한다. 

![[Pasted image 20231018165042.png | 500]]
<div align="center">Euclidean Algorithm</div>

위 식을 [[Modular Arithmetic]]을 이용해 간단하게 표기하면 $gcd(a, b) = gcd(b, a \bmod b) \; where \; a>b$라고 쓸 수 있고, 슈도 코드로 작성하면 다음과 같다
```pseudo code
Euclidean Algorithm(a, b):
	A = a, B = b
	while B > 0
		R = A mod B
		A = B, B = R
	return A
```

