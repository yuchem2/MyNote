> If $a$ is an integer and $n$ is a positive integer, we define $a \bmod n$ to be the remainder when $a$ is divided by $n$The integer $n$ is called the *modulus*. 

위 정의에 따라 [[Divisibility]]에서 정의한 식을 다음과 같이 다시 쓸 수 있다.$$\begin{align}a& =qn+r\qquad \qquad 0 \leq r <n; \; q=\lfloor a/{n} \rfloor \\ a& = \lfloor a/{n} \rfloor \times n + (a \bmod n)\end{align}$$
또한 정수 $a, b$에 대해서 $a\bmod n = b \bmod n$인 경우 두 정수는 congruent modulo $n$이라고 할 수 있으며 다음과 같이 표기한다. $$a\equiv b\pmod n$$
또한, 만약 $a=0\pmod n$이면 $n|a$이다

## Properties of Congruences
1. $a \equiv b \pmod n\; if \; n|(a-b)$
2. $a \equiv b \pmod n\; implies \; b = a \pmod n$
3. $a \equiv b \pmod n \; and \; b \equiv c \pmod n \; imply \; a \equiv c \pmod n$

## Operations
1. $[(a \bmod n) + (b \bmod n)] \bmod n = (a+b) \bmod n$
2. $[(a \bmod n) - (b \bmod n)] \bmod n = (a-b) \bmod n$
3. $[(a \bmod n) \times (b \bmod n)] \bmod n = (a \times b) \bmod n$

## Properties
Define the set $Z_n$ as the set of nonnegative integers less than $n$: $$Z_n = \{0, 1, ..., (n-1)\}$$위와 같은 집합을 *set of residues* 혹은 *residue classes*$\pmod n$로 정의한다. 집합의 각 원소를 다음과 같이 라벨링 할 수 있다. $$\begin{align}Can \; label \; the \; residue\; classes\; \pmod n\; as& \;[0], [1], [2], ..., [n-1]: \\ &[r] = \{a: a \; is \; an \; integer,\; a \equiv r\pmod n\}\end{align}$$residue classes의 모든 정수 중에서 가장 작은 음이 아닌 정수는 residue classes를 대표한다. $k$가 congruent modulo $n$인 가장 작은 음이 아닌 정수를 찾는 것을 reducing $k$ modulo $n$이라 한다

이 $Z_n$에 연산을 수행하게 되면 [[Ring#Commutative Ring]]이라는 것을 알 수 있다. 즉, 다음과 같은 성질을 만족한다. 
1. 곱셈과 덧셈에 대해서 닫힘 성질을 가진다.
2. 곱셈과 덧셈에 대해서 교환 법칙이 성립한다
3. 곱셈과 덧셈에 대해서 분배 법칙이 성립한다
4. 곱셈과 덧셈에 대해서 항등원이 존재한다
5. 뎃셈에 대해서 역원이 존재한다. 
6. [[GCD(Greatest Common Divisor)#Relatively Prime]]의 규칙을 적용하면 곱셈 역원이 성립하는 듯 보이나 오직 곱셈 역원이 존재함을 밝히고자 하는 $a$와 $n$이 relatively prime인 경우에만 모든 경우에서 성립하는 것을 볼 수 있다. 만약 두 수가 relatively prime이 아닌 경우에는 곱셈 역원이 유일하지 않다. 