> A polynomial of degree $n$ (integer $n\geq 0$) is an expression of the form $$f(x) = a_nx^n + a_{n-1}x^{n-1} + ... + a_1x + a_0 = \sum_{i=1}^na_ix^i$$where the $a_i$ are elements of some designated set of numbers $S$, called the coefficient set, and $a\neq0$

## 종류
---
### Ordinary Polynomial 
어떠한 제한도 없는 다항식으로 일반적으로 polynomial이라고 하면 이 형태를 말한다. 덧셈, 뺄셈, 곱셈, 나눗셈이 정의되어 있다. 즉, [[Field]]이다. 

### Polynomial with coefficients in $Z_p$
계수가 $\pmod p$에 갇혀 있는 형태이다. 하지만 이렇게만 정의하는 경우이러한 polynomial은 [[Ring]]이 되게 된다. 일반적인 나눗셈 방법으로는 모든 경우에서 곱셈 역원을 정의할 수 없기 때문이다. 그러므로 모든 polynomial의 형태를 다음과 같이 정의한다. $$f(x) = q(x)g(x) + r(x)$$
with polynomial degress:
	Degree $f(x) = n$
	Degree $g(x) = m$
	Degree $q(x) = n-m$
	$0\leq$ Degree $r(x) \leq m-1$
이렇게 표기한다면 moduler연산을 다음과 같이 표현할 수 있다. $$r(x) = f(x) \bmod g(x)$$
여기서 $g(x)$가 prime과 같은 성질(only $g(x)|g(x)$ and $1|g(x)$)인 경우에 *irreducible(or prime) polynomial*이라고 한다. 즉, 계수가 $\pmod p$에 닫혀있고, polynomial이 $\pmod {g(x)}$에 닫혀 있는 경우 [[Field]]라고 할 수 있다. 즉, 곱셈 역원이 정의된다.

여기서 p는 어떠한 소수로 결정해도 상관없다. 하지만 [[암호화 알고리즘(Cypotographic algorithms)]]에서는 $p=2$인 경우에 관심이 있다. [[Finite Field#GF($2 n$)]]와 동일한 표현으로 사용되고 다음과 같이 연산을 정의할 수 있다. $$\begin{align} addition: &\; f_1(x) + f_2(x) = (f_1(x)+f_2(x))\bmod g(x) \\ multiplication: & \;f_1(x) \times f_2(x) = (f_1(x) \times f_2(x)) \bmod g(x)\end{align}$$

![[Pasted image 20231020160349.png | 700]]
<div align="center">Polynomial Arithemtic Modulo(x^3+x+1)</div>

