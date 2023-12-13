알고리즘에 의해 생성된 고정된 작은 크기의 데이터 블록으로, cryptographic checksum 혹은 MAC이라고 불린다. 이 블록은 [[Message Authenication]]을 할 수 있도록 제공한다.

이 블록은 메시지와 설정된 key값에 의해 결정된다. 암호화와 유사하지만, 역 연산을 필요로 하지 않는다. 송신 측에서 메시지와 MAC을 함께 보내고, 수신 측에서는 메시지에 대해 연산을 수행해 그 결과가 보내진 MAC과 같은 지를 확인하는 작업을 수행한다. 

이를 통해 수신자는 예상한 송신자로부터 메시지가 왔음을 확인할 수 있고, 또한 메시지가 바뀌지 않았다는 것을 확인할 수 있다. 

위에서 언급한 대로 MAC은 [[인증(Authentication)]]을 제공한다. 추가적으로 보안을 위한 암호화를 하는데 사용될 수 있다. 

[[Digital Signature]]과 유사하지만, [[Digital Signature]]는 아니다. 
## Generate MAC
통신하는 두 객체 $A, B$가 존재하고, 두 객체가 공유 키 $K$를 공유하고 있다고 가정하고, $A$가 $B$에게 메시지 $M$을 보낼 때 MAC은 다음과 같이 생성된다. $$MAC = C(K, M)$$여기서 C()은 MAC을 생성하는 함수이다. 이 함수는 many-to-one function으로 [[Hash Function]]과 유사하다. 그러므로 여러 메시지가 같은 MAC을 가질 수 있다. 그러나 이러한 메시지들을 찾는 것은 매우 어렵다. 

![[Pasted image 20231206154312.png | 600]]
## Requirements
MAC에는 다음과 같은 요구사항들이 존재한다. 
+ 여러가지 타입의 [[Security attack]]을 대항할 수 있어야 한다.
+ 메시지와 MAC을 알 때 같은 MAC을 만들어내는 메시지를 찾는 것이 매우 어려워야 한다. ([[Cryptographic Hash Function]]의 weak collision과 유사)
+ MAC 값은 [[Uniform distribution]]으로 분포해 있어야 한다.
+ 메시지의 모든 bit로부터 동일하게 영향을 받는다. 
## [[HMAC(Hash MAC)]]
![[HMAC(Hash MAC)]]
## [[CMAC]]
![[CMAC]]
## Authenticated Encryption
기밀성과 통신의 신뢰성을 만족시키는 방법이다. 많은 프로그램이 이를 지원하고 있지만 일반적으로 두 부분이 나눠서 설계된다. 일반적으로 다음과 같은 4가지 접근이 존재한다.
+ Hashing followed by encryption($H\rightarrow A$): $E(K, (E\;||\;H(M))$
+ Authentication followed by encryption($A \rightarrow E$): $E(K2, (M\; ||\; MAC(K1, M))$
+ Encryption followed by authenticaion($E\rightarrow A$): $C=E(K2, M), \; T=MAC(K1, C)$
+ Independently encrypt and authenticate($E+A$): $C=E(K2, M), \; T=MAC(K1, M)$

각 접근에서 복호화와 검증은 다음과 같이 이루어진다. $H\rightarrow A$, $A \rightarrow E$, $E+A$는 복호화를 먼저 진행한 후에 검증을 진행하게 된다. $E\rightarrow A$는 검증을 먼저 한 후에 복호화를 진행한다. 

위 접근 방식은 모두 보안 취약성이 존재한다. 
### Couter with [[CBC(Cipher Block Chaining)]] MAC
CCM 모드로 불리는 이 연산은 WiFi에서 사용되는 NIST 표준이다. 암호화와 MAC을 함께 이용하는 방식이다. 암호화와 MAC에 동일한 키를 사용한다. 여기서 사용되는 알고리즘은 다음과 같다.
+ [[AES(Advanced Encryption Standard)]]
+ [[CTR(Counter)]] 
+ CMAC

![[Pasted image 20231207103349.png | 600]]
