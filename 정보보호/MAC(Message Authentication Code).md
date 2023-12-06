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
## HMAC
MAC을 [[Cryptographic Hash Function]]을 이용해 생성하는 알고리즘을 말한다. 이 형태가 사용되는 이유는 [[Hash Function]]은 일반적으로 매우 빠르고, [[Cryptographic Hash Function]]는 다양한 곳에서 사용 가능하기 때문이다. 

일반적인 [[Cryptographic Hash Function]]와 다르게, 키 $K$와 메시지 $M$을 함께 사용해 hash value를 생성하며 이를 keyed hash라고 부른다. $$KeyedHash = Hash(K|M)$$
### Desgin Objectvies
+ 이용 가능한 [[Hash Function]]을 수정 없이 사용
+ 더 빠르고 효과적인 [[Hash Function]]이 나온 경우 내장된 [[Hash Function]]를 쉽게 교체 가능
+ 원본 [[Hash Function]]의 성능의 저하가 발생하지 않아야 한다
+ 키를 사용하고 관리하는 것이 간단해야 한다
+ 내장된 [[Hash Function]]에 대한 메커니즘의 강도가 예상한 대로 이루어져야 함.
### Algorithm
인터넷 표준 RFC2104에 기술되어 있는 알고리즘. 먼저 다음과 같은 변수를 정의할 수 있다. 
+ $H$: 내장된 [[Hash Function]] (e.g. MD5, SHA-1, RIPEMD-160)
+ $IV$: [[Hash Function]]에 입력으로 들어갈 초기 값
+ $M$: HMAC에 입력으로 들어온 메시지
+ $Y_i$: 메시지의 $i$ 번째 블록
+ $L$: 메시지 블록의 수
+ $b$: 각 블록의 bit 수
+ $n$: 내장된 [[Hash Function]]으로부터 생성되는 hash value의 길이
+ $K$: 비밀키로, $n$보다 긴 길이가 추천된다. 만약 키의 길이가 $b$보다 큰 경우 키는 $n$ 비트 키를 생성하기 위해 [[Hash Function]]에 입력된다.
+ $K^+$: 길이가 $b$가 되도록 비밀 키 값에 zero paddding을 수행한 결과
+ $ipad$: 00110110($=36_{(16)}$) 값이 $b/8$ 번 씩 반복되는 2진수
+ $opad$: 01011100($=5C_{(16)}$) 값이 $b/8$ 번 씩 반복되는 2진수

이 값들을 통해 HMAC은 다음과 같이 정의된다. $$HMAC(K, M) = H[(K^+\oplus opad)\; || \; H[(K^+\oplus ipad) \;|| \;M]]$$![[Pasted image 20231206161938.png|600]]
### Security
HMAC의 보안성은 내장한 [[Hash Function]]에 의존하고 있다. HMAC에 대해 다음과 같은 공격이 존재할 수 있다.
+ [[Brute-force attack]]
+ birthday attack

또한, [[Hash Function]]을 선택할 때 속도와 보안성 중 어떤 것을 중요하게 생각하는 지에 따라 결과가 다를 수 있다. 
## CMAC
[[Symmetric encryption]]을 기반으로 사용하는 방식으로, [[Block Cipher]]에서 사용하는 기법 중 하나인 [[CBC(Cipher Block Chaining)]]를 이용한다. 연산의 마지막 블록을 MAC으로 사용하는 것이다.
### [[DAA(Data Authentication Algorithm)]]
![[DAA(Data Authentication Algorithm)]]
### CMAC
[[DAA(Data Authentication Algorithm)]]의 결과의 길이가 매우 짧다는 단점을 해결하기 위해 등장한 방법이다. 이를 해결하기 위해 메시지 길이는 제한을 두게 되었지만 암호성을 획득할 수 있게 되었다. 이 방법에서는 2개의 키와 padding을 사용한다. 

![[Pasted image 20231206163224.png | 600]]

## Authenticated Encryption
