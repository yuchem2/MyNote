
보호할 데이터와 비밀 키를 입력으로 받아 데이터를 변환해 암호문으로 변환하는 [[암호화 알고리즘(Cypotographic algorithms)]] 

일반적인 암호화를 말하며 Single-key 암호화라고도 한다. 

1970년대 public key 암호화의 개발 이전에 사용되던 유일한 암호화 유형이다. 이로 인해 고전 암호라고도 한다.

데이터를 처리하는 방법에 따라 구분된다
+ [[Block Cipher Symmetric Encryption]]
+ [[Stream Cipher Symmetric Encryption]]

## Ingredients
---
+ Plaintext(평문): 원본 메시지나 데이터를 말하며 암호화 알고리즘의 입력 값이다.
+ Encryption algorithm: 평문에 대해 한 번 이상의 substitution과 transformations을 수행하는 알고리즘을 말함
+ Secret key: 암호화 알고리즘의 다른 입력 값으로, 평문에 종속적인 형태를 띈다. 암호화 알고리즘은 특정 키에 따라 다른 출력 값을 나타내며 이는 substitution과 transformation이 키에 의존적임을 의미한다
+ Ciphertext(암호문): 암호화 알고리즘의 출력 값으로, 평문과 비밀키에 종속적이다. 
+ Decrpytion algorithm: 암호화 알고리즘의 역순으로 진행되는 알고리즘으로 암호과 비밀 키를 이용해 평문을 만든다. 

평범한 암호화의 경우 다음과 같은 요구사항이 존재한다. 
+ 강력한 암호화 알고리즘이 필요하다. 이는 많은 암호문, 평문 쌍에 대한 정보를 가지고 있다고 해도 키를 발견할 수 없고, 암호문을 복호화할 수 없는 알고리즘을 말한다
+ 송수신자 모두 비밀키에 대한 정보를 가지고 있어야 하며 이를 보호된 상태로 유지해야한다.

## Model
---
![[Pasted image 20231005180415.png | 650]]

일반적인 대칭 암호화 모델은 위와 같은 형식을 띈다. 
암호화 알고리즘 $E(K, X)$에 의해 $Y$라는 암호문이 만들어지게 된다. 또한, 복호화 알고리즘 $D(K, Y)$에 의해 $X$라는 평문이 만들어지고 이를 수식으로 표현하면 아래와 같다. $$Y = E(K, X)$$ $$X = D(K, Y)$$
## Example
---
+ [[Substitution Ciphers]]
	+ [[Caesar Cipher]]
	+ [[Mono-alphabetic Cipher]]
	+ [[Playfair Cipher]]
	+ [[Poly-alphabetic Cipher]] 
		+ [[Vigenere Cipher]]
		+ [[Autokey Cipher]]
		+ [[One-Time Pad]]
+ [[Transposition Ciphers]]
	+ [[Rail Fence Cipher]]
	+ [[Row Transposition Cipher]]
+ [[Product Ciphers]]
	+ [[Rotor Machines]]
+ [[Steganography]]
