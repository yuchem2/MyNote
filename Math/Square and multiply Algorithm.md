거듭 제곱을 위한 효율적이고 빠른 [[알고리즘(Algorithm)]]으로, 반복적인 squaring을 통해 수행하는 것을 기초로 한다. 

$a, b, m$이 양의 정수일 때 $a^b \bmod n$인 값을 찾고자 하는 경우 다음과 같이 수행할 수 있다. 먼저 $b$를 이진수로 표현하면 $b_kb_{k-1}...b_0$이고,  $$b=\sum_{b_i\neq0}2^i$$라고 할 수 있다. 그러면, $$\begin{align}a^b&=a^{(\sum_{b_i\neq0}2^i)} = \prod_{b_i\neq0} a^{(2^i)}\\a^b\bmod n &=\left[ \prod_{b_i\neq0}a^{(2^i)} \right] \bmod n = \left(\prod_{b_i\neq0}\left[a^{(2^i)}\bmod n\right]\right)\bmod n\end{align}$$
이를 알고리즘으로 표현하면 다음과 같다.  
```pseudo code
c := 0; f := 1
for i := k downto 0
	c := 2 x c
	f := (f x f) mod n
	if bi = 1
		c := c + 1
		f := (f x a) mod n
return f
```

e.g. $a=7, \; b=560\; 1000110000,\; n=561$일 때 알고리즘은 다음과 같이 진행한다.

|   i   |  9  |  8  |  7  |  6  |  5  |  4  |  3  |  2  |  1  |  0  |
|:-----:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| $b_i$ |  1  |  0  |  0  |  0  |  1  |  1  |  0  |  0  |  0  |  0  |
|   c   |  1  |  2  |  4  |  8  | 17  | 35  | 70  | 140 | 280 | 560 |
|   f   |  7  | 49  | 157 | 526 | 160 | 241 | 298 | 166 | 67  |  1  |  
