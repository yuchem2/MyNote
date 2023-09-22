

가역적인 알고리즘과 비가역적인 알고리즘으로 구분할 수 있다

가역적인 암호화 알고리즘은 단순히 데이터가 암호된 후에 복호화되도록 허용하는 알고리즘으로 암호 Key가 존재한다(Single-key, Two-key)

비가역적인 암호화 알고리즘은 암호 Key가 따로 존재하지 않는다(Keyless). [[디지털 서명(Digital Signature)]]과 message authenication에서 사용된다.


## **분류**
---
### Keyless
결정론적인 함수들(Deterministic functions)을 사용함으로써 암호화하는 알고리즘
+ [[Cryptographic hash function]]
+ [[PRNG(Pseudo-Random Number Generator)]]

### Single-Key
하나의 사용자 비밀 키를 이용한 알고리즘
+ [[Symmetic encryption]]
	+ [[Block cipher symmetic encryption]]
	+ [[Stream cipher symmetric encryption]]
+ [[MAC(Message Authentication Code)]]

### Two-key
두 개의 사용자 비밀 키를 이용한 알고리즘. Private key와 public key로 나눠진다. 
+ [[Asymmetric encryption]]
+ [[디지털 서명(Digital Signature)]]
+ [[Key exchange]]
+ [[User authentication]]