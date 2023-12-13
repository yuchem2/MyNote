[[MAC(Message Authentication Code)]]을 [[Cryptographic Hash Function]]을 이용해 생성하는 알고리즘을 말한다. 이 형태가 사용되는 이유는 [[Hash Function]]은 일반적으로 매우 빠르고, [[Cryptographic Hash Function]]는 다양한 곳에서 사용 가능하기 때문이다. 

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