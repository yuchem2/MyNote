
입력 데이터를 블럭 단위로 쪼개어 암호화하는 [[암호화 알고리즘(Cypotographic algorithms)]]
암호화를 진행해 나오는 암호문은의 길이는 항상 입력으로 들어온 평문의 길이와 동일하다
블럭의 크기는 일반적으로 64 bits이나 128bits을 사용한다
특정한 기능을 추가해 마치 [[Stream Cipher]]와 같은 효과를 달성할 수 있다 ^358fa4
+ [[Block Cipher Symmetric Encryption]]
	+ [[Feistel Cipher]]
	+ [[DES(Data Encryption Standard)]]
	+ [[AES(Advanced Encryption Standard)]]
+ [[Block Cipher Asymmetric Encryption]]


![[Pasted image 20231012193806.png | 600]]


## Design Principles
기본적인 원칙은 [[Feistel Cipher]]과 유사하다. 
+ number of rounds: 수가 많을 수록 좋다.
+ function $f$: *Confussion*을 제공하고, 비선형적이며 [[Avalanche Effect]]을 발생시킨다
+ key schedule: 복잡한 subkey 생성을 하며 key [[Avalanche Effect]]을 발생시킨다 


## Modes of Operation
[[Block Cipher]]는 입력된 평문을 고정된 블럭 크기로 잘라 암호화를 하는 암호이다. 이때 일반적으로 입력된 평문을 여러 개의 블럭으로 나눈 후 같은 비밀 키를 이용해 각 블럭을 암호화하게 된다. 이로 인해 블럭 내용이 반복적이면 쉽게 암호 키를 유추할 수 있는 문제가 등장하였다. 이를 해결하기 위해 NIST에서 이를 해결하기 위해 [[Block Cipher]]에 적용할 수 있는 5가지 모드를 제시하였다. 크게 block mode와 stream mode로 나눠 볼 수 있다. 

![[Pasted image 20231022144620.png | 600]]
<div align="center">Block Cipher Modes of Operations</div>

### Block Mode
![[ECB(Eletronic Codebook)]]

![[CBC(Cipher Block Chaining)]]

### Stream Mode
[[Block Cipher]]를 마치 [[Stream Cipher]]처럼 작동하게 만드는 모드

![[CFB(Cipher Feedback)]]

![[OFB(Output FeedBack)]]

![[CTR(Counter)]]

### Feedback Characteristics

![[Pasted image 20231022155051.png | 600]]
<div align="center">Feedback Characteristic of Modes of Operation</div>
