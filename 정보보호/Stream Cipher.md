
입력 데이터를 데이터 스트림 비트 혹은 바이트로 쪼개어 암호화하는 [[암호화 알고리즘(Cypotographic algorithms)]]

데이터의 총 길이에 대해 같은 길이의 비밀 키를 생성해 암호화를 진행하기 때문에 비밀 키의 길이가 매우 긴 형태를 가진다. 이때 비밀 키는 [[PRNG(Pseudo-Random Number Generator)]]를 이용해 생성하게 된다.

보통 일반적으로 평문과 만들어진 암호키를 XOR연산해 암호문을 생성한다 

+ [[Stream Cipher Symmetric Encryption]]
	+ [[RC4]]
+ [[Stream Cipher Asymmetric Encryption]]

![[Pasted image 20231012193746.png | 600]]

## Design Principles
---
설계를 할 때 고려되는 사항들은 다음과 같다.
+ 반복 없이 오랜 기간 사용될 수 있어야 한다
+ 통계적으로 랜덤해야 한다
+ 충분하고 큰 키에 의존한다
+ 높은 선형 복잡도를 가져야한다
+ [[Block Cipher]]와 같은 크기의 키를 사용할 때만큼 안전해야 하고, 일반적으로 간단하고 빠르다. 