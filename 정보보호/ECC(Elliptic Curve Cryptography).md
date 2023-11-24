## [[Elliptic Curve]]
![[Elliptic Curve]]
## Background
[[RSA]]와 [[Diffie-Hellman Key Exchange]] 모두 매우 큰 수의 숫자나 다항식을 요구하고 있다. 이로 인해 key를 생성하는 과정의 부하가 매우 높다. 이로 인해 두 알고리즘과 동일한 보안성을 제공하면서도 비교적으로 작은 단위의 bit 크기를 사용하는 암호의 개발이 필요해졌다. 
## Algorithm
EEC 덧셈은 modulo에서 곱셈과 유사하고, ECC 곱셈은 modulo에서 거듭제곱과 유사하다. 
여기서 암호화는 $Q=kP, \; where \; Q, P\; belong \; to \; a \; prime \; curve$를 통해 이루어진다.
+ $k, P$를 알고 있으면 $Q$를 계산하는 것은 매우 쉽다
+ $Q,P$를 알고 있을 때 $k$를 계산하는 것은 매우 어려움

즉, $k$를 private key로, $Q$와 $P$를 public key로 사용한다면 보안성을 유지할 수 있다. 
e.g. $E_{23}(9, 17)$
### ECC [[Diffie-Hellman Key Exchange]]
