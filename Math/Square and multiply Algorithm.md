거듭 제곱을 위한 효율적이고 빠른 [[알고리즘(Algorithm)]]으로, 반복적인 squaring을 통해 수행하는 것을 기초로 한다. 

$a, b, m$이 양의 정수일 때 $a^b \bmod n$인 값을 찾고자 하는 경우 다음과 같이 수행할 수 있다. 먼저 $b$를 이진수로 표현하면 $b_kb_{k-1}...b_0$이고,  $$b=\sum_{b_i\neq0}2^i$$라고 할 수 있다. 그러면, $$\begin{align}a^b&=a^{(\sum_{b_i\neq0}2^i)} = \prod_{b_i\neq0} a^{(2^i)}\\a^b\bmod n &=\left[ \prod_{b_i\neq0}a^{(2^i)} \right] \bmod n = \left(\prod_{b_i\neq0}\left[a^{(2^i)}\bmod n\right]\right)\bmod n\end{align}$$
e.g. $7^9 = 7^8 \times 7^1 = ((7\times7)\times7^4)\times7^1 = (5\times 7^2\times 7^2)\times7^1$  