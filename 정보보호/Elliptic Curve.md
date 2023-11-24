## Elliptic Curves over Real Numbers
두 개의 변수 $x, y$를 통해 elliptic curves를 정의할 수 있다. $$y^2 =x_3 + ax+b$$이 식에서 $x, y, a, b$는 모두 실수의 범위에 속하며 0이 될 수도 있다.

Elliptic Curve의 정의에서 $a, b$의 값을 변경하면 나올 수 있는 $x, y$의 범위를 결정할 수 있고 이러한 두 쌍을 $E(a, b)$라고 결정해 사용한다. 
### Example
![[Pasted image 20231124162614.png | 600]]
## Elliptic Curves over [[Finite Field]]
[[ECC(Elliptic Curve Cryptography)]]에서 사용하는 [[Elliptic Curve]]이다. 일반적으로 다음과 같은 범위에서 정의되는 두 형태가 존재한다.
+ $Z_p$에서 정의되는 prime curves $E_p(a, b)$
+ $GF(2^m)$에서 정의되는 binary cureves $E_{2m}(a, b)$