데이터의 수신자가 데이터의 출처와 무결성을 증명하고 위, 변조를 방지할 수 있도록 데이터에 첨부되거나 암화화된 데이터 

[[Message Authenication]]을 통해서 [[인증(Authentication)]]이 제공될 수 있으나 여전히 신뢰가 부족할 수 있다. 이때 [[Digital Signature]]를 사용한다. 

[[Digital Signature]]는 메시지의 주체를 확인할 수 있게 하며 추가적으로 생성 시기 또한 알 수 있다. 아래 그림과 같은 형태로 서명을 추가하고, 서명을 검증할 수 있다. 일반적으로 [[Asymmetric Encryption]] 방식을 사용한다. 

![[Pasted image 20231207104629.png | 600]]

## Attacks and Forgeries
디지털 서명에 가능한 공격과 위조가 존재한다. 먼저 서명을 보낼 때 가능한 공격은 다음과 같다. 
+ Key-only attack: 공격자가 서명을 한 사람의 public key를 알고 있음
+ Known message attack: 공격자가 서명과 메시지에 접근 가능
+ Generic chosen message attack: 공격자는 공격할 서명 방법을 확인하기 전에 메시지 리스트를 선택해 놓는다. 이때 선택하는 메시지들은 서명한 사람의 public key와 관련이 없다. 그리고, 공격자는 선택한 메시지들 중 유효한 서명을 얻게 된다. 
+ Directd chosen message attack: 공격할 사람의 public key를 알고 있는 상태에서 메시지 리스트를 선택한다. 
+ Adaptive chosen message attack: 공격자가 공격할 사람의 서명을 요청하며 메시지를 선택한다. 

이러한 공격이 성공하게 되면 다음과 같은 위조를 시도할 수 있다. 
+ Total break: 공격자는 공격한 사람의 private key를 결정할 수 있다
+ Universal forgery: 공격자는 임의의 메시지에 서명을 구성하는 동등한 방법을 제공하는 효율적인 서명 알고리즘을 찾을 수 있다
+ Selective forgery: 공격자가 선택한 특정 메시지에 대한 서명을 위조한다.
+ Existential forgery: 적어도 하나의 메시지에 대한 서명을 위조한다. 공격자는 메시지에 대한 제어권이 없다. 결과적으로 이 위조는 피해자에게 사소한 것일 수 있다
## Requirement
[[Digital Signature]]에서 충분한 보안성을 제공하기 위해 필요한 요구사항은 다음과 같다. 
+ 메시지에 의존적이여야 한다
+ 전송자의 유일한 정보를 사용해야 한다. (위조와 부정을 막기 위해)
+ 생성하는 과정이 비교적 쉬워야 한다.
+ 인식과 검증이 비교적 쉬워야 한다.
+ 위조하는 것은 계산적으로 불가능해야 한다. (새로운 메시지-존재하는 서명, 주어진 메시지로 임의의 서명을 생성)
+ 사본을 저장하려면 실용적이여야 한다
## 종류
### Direct Digital Signature
통신하는 두 객체 간에서만 유효한 서명을 생성하는 방법이다. 

수신자가 송신자의 public key를 가지고 있는 상황을 가정한다. 
1. 송신자는 전체 메시지 혹은 메시지의 hash value를 private key를 이용해 서명한다.
2. 보낼 때 수신자의 public key로 암호화해 보낼 수 있다.
3. 받은 수신자는 자신의 private key로 복호화하고, 서명을 송신자의 public key를 이용해 검증할 수 있다. 
### Arbitrated Digital Signature
중재자 A를 통해서 서명을 생성하는 방법이다. 이 방법을 사용하기 위해서는 신뢰할 수 있는 중재자가 필요하다. 이 방법에서는 private, public key 둘 다 사용될 수도 있다. 

이 방법에서는 자신의 서명을 중재자를 통해 생성하게 된다. 이때 중재자는 메시지를 확인할 수도 있고 확인하지 않을 수도 있다. 

## Algorithms
### [[ElGamal Digital Signatures]]
![[ElGamal Digital Signatures]]
### [[Schnorr Digital Signatures]]
![[Schnorr Digital Signatures]]
### DSS(Digital Signature Standard)
미국 정부에서 사용하는 방법으로 NIST & NSA에 의해 1990년대 초반에 설계되었다. [[SHA(Secure Hash Algorithm)]]을 사용하는 방법으로 이 방법의 알고리즘을 DSA라고 명시한다. 2000년에 [[RSA]]와 [[Elliptic Curve]]을 사용하는 방법이 추가되었다.
#### DSA(Digital Signature Algorithm)
320 bit 크기의 서명을 생성한다. 512~1024 bit의 prime number를 사용하기 때문에 이 크기만큼의 보안성을 갖는다. [[RSA]]보다 작으며 빠르다. 

이 알고리즘은 오직 서명에 대한 방법만 제공한다. 

![[Pasted image 20231207114227.png | 600]]
##### Key Generation
Global prameter $(p, q, g)$를 사용한다. 
+ Prime number $p=2^L$($L=512-1024 \;bits$)
+ $p-1$의 prime factor인 $160\; bit$의 $q$를 선택
+ $g=h^{{p-1}/q} \; where \; h < p-1, \;h^{p-1/q} \bmod p > 1$ 

각 사용자는 global parameter를 사용해 각자의 키를 생성한다.
+ private key: chose $x<q$
+ public key: $y=g^x \bmod q$
##### Digital Signature Creation
송신자는 메시지 $m$에 서명을 진행한다.
+ 임의의 서명 키 $k$를 생성한다. ($k<q$)
+ 이때 $k$는 반드시 임의의 수여야 하며 사용한 뒤에 삭제되어야 한다. 또한, 중복된 $k$가 사용되서는 안된다.
+ $k$로부터 서명을 생성한다
	+ $r=(g^k \bmod p) \bmod q$
	+ $s = k^{-1} (SHA(M)+x\times r) \bmod q$
+ ($r, s$)를 메시지와 함께 전송한다. 

##### Digital Signature Verification
메시지 $m$과 서명 $(r, s)$를 수신 후 서명을 검증한다. 
+ $w = s^{-1} \bmod q$
+ $u_1 = (SHA(M)\times w)\bmod q$
+ $u_2 = (r\times w) \bmod q$
+ $v=(g^{u_1} \times y^{u_2} \bmod p) \bmod q$

위에서 계산된 $v=r$이면, 옳은 서명이 도착했음을 판단할 수 있다. 
![[Pasted image 20231212093140.png | 600]]
