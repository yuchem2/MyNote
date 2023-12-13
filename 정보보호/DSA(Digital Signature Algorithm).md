320 bit 크기의 서명을 생성한다. 512~1024 bit의 prime number를 사용하기 때문에 이 크기만큼의 보안성을 갖는다. [[RSA]]보다 작으며 빠르다. 

이 알고리즘은 오직 서명에 대한 방법만 제공한다. 

![[Pasted image 20231207114227.png | 600]]
### Key Generation
Global prameter $(p, q, g)$를 사용한다. 
+ Prime number $p=2^L$($L=512-1024 \;bits$)
+ $p-1$의 prime factor인 $160\; bit$의 $q$를 선택
+ $g=h^{{p-1}/q} \; where \; h < p-1, \;h^{p-1/q} \bmod p > 1$ 

각 사용자는 global parameter를 사용해 각자의 키를 생성한다.
+ private key: chose $x<q$
+ public key: $y=g^x \bmod q$
### Digital Signature Creation
송신자는 메시지 $m$에 서명을 진행한다.
+ 임의의 서명 키 $k$를 생성한다. ($k<q$)
+ 이때 $k$는 반드시 임의의 수여야 하며 사용한 뒤에 삭제되어야 한다. 또한, 중복된 $k$가 사용되서는 안된다.
+ $k$로부터 서명을 생성한다
	+ $r=(g^k \bmod p) \bmod q$
	+ $s = k^{-1} (SHA(M)+x\times r) \bmod q$
+ ($r, s$)를 메시지와 함께 전송한다. 
### Digital Signature Verification
메시지 $m$과 서명 $(r, s)$를 수신 후 서명을 검증한다. 
+ $w = s^{-1} \bmod q$
+ $u_1 = (SHA(M)\times w)\bmod q$
+ $u_2 = (r\times w) \bmod q$
+ $v=(g^{u_1} \times y^{u_2} \bmod p) \bmod q$

위에서 계산된 $v=r$이면, 옳은 서명이 도착했음을 판단할 수 있다. 
![[Pasted image 20231212093140.png | 600]]
