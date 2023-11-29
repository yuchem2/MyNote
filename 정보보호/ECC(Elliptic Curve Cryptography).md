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
[[Diffie-Hellman Key Exchange]]과 유사한 방식으로 키 교환을 수행할 수 있다. 
1. 유저들은 적절한 [[Elliptic Curve]] $E_q(a, b)$를 선택
2. [[Prime Number#Primitive Roots]]를 선택하는 것처럼 base point $G=(x_1, y_1)$을 선택. 이때 매우 큰 수 $n$에 대하여 $nG=0$을 만족해야 한다.
3. A와 B는 각각 private key를 선택: $n_A<n, \; n_B<n$
4. 각자의 public key를 계산: $P_A = n_AG, \; P_B = n_BG$
5. 공유키는 다음과 같이 계산: $K=n_AP_B,\; K=n_BP_A$ 즉, $K=n_An_BG$ 
### ECC Encryption/Decryption
1. 메시지 $M$을 [[Elliptic Curve]]의 point $P_m$가 되도록 encode
2. 위 경우와 동일하게 적절한 $E_q(a, b)$와 base point $G=(x_1, y_1)$을 선택
3. B는 private key $n_B<n$를 선택한 뒤 public key $P_B = n_BG$를 계산한 후 알림
4. A는 encode된 메시지 $P_m$을 B에게 보내기 위해 encryption: $C_m = \{kG, \; P_m+kP_B\}$ 여기서 $k$는 랜덤한 수를 선택
5. B는 전달받은 $C_m$을 decyption: $P_m + kP_B - n_B(kG) = P_m + k(n_BG) - n_B(kG) = P_m$
## Security
보안성 정도는 [[Elliptic Curve]] lograrithm problem에 의존한다. 이 문제를 해결하는 가장 최근 기술은 Pollard rho method가 존재한다.

[[RSA]]에 비해 매우 작은 크기의 키 사이즈를 사용할 수 있다는 점에서 장점을 가진다. 아래 표를 통해 [[ECC(Elliptic Curve Cryptography)]]가 유사한 다른 알고리즘들에 비해 계산에 장점이 있음을 확인할 수 있다.

![[Pasted image 20231129160024.png | 600]]
<div align="center">Comparable Key Sizes in Terms of Computational Effort for Cryptonalysis</div>