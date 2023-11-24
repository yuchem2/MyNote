Private key와 Public key 두 개의 키를 사용하는 [[암호화 알고리즘(Cypotographic algorithms)]]
Public-Key encryption

기존 [[Symmetric encryption]]에서는 Private key 한 개만을 사용하며 permutation 연산과 substitution 연산을 통한 대칭적인 암호화, 복호화를 수행한다. 이 형태를 완전히 대치하기 위해 등장한  [[Asymmetric encryption]]은 두 개의 키를 사용하며 수학적인 함수를 비대칭적인 암호화, 복호화를 수행한다. 

[[Symmetric encryption]]보다 좀 더 보안성이 높다.

[[Key exchange]]와 [[디지털 서명(Digital Signature)]] 문제를 해결하기 위해 개발되었다. *Diffie*와 *Hellman*에 의해 최초로 공개된 형태이다. 

데이터를 처리하는 방법에 따라 구분된다.
+ [[Block Cipher Asymmetric Encryption]]
+ [[Stream Cipher Asymmetric Encryption]]
## Ingredients
+ Plaintext
+ Encryption algorithm
+ Public and private keys
	+ Public-key: 모두에게 공개된 키로, 암호화 혹은 서명 검증을 할 때 사용
	+ Private-key: 자신에게만 알려진 키로, 복호화 혹은 서명하거나 서명을 만들 때 사용
+ Ciphertext
+ Decryption algorithm
## Model
![[Pasted image 20231117154704.png | 600]]
일반적인 공용키를 통해 암호화는 위와 같은 형식을 띈다. 위의 형식은 수신자의 공용키를 통해 암호화를 진행하고, 수신자는 자신의 비밀키를 통해 복호화를 수행한다. $$\begin{align}Y = &E(PU_a, X)\\ X =&D(PR_a, Y)\end{align}$$
## Characteristics
+ *Computationally infeasible(difficult)* to find decrypthon key knowing only algorithm & encrypthon key
+ *Computationally easy* to en/decrypt messages when the relevant(en/decrypt) key is known
+ either of the two related keys can be used for encryption, with the other used for decryption(in some schemes)
## Applications
+ encryption/decryption
+ [[디지털 서명(Digital Signature)]]
+ [[Key exchange]]

|     algorithm      | Encryption/Decryption | Digital Signature | Key Exchange |
|:------------------:| --------------------- | ----------------- | ------------ |
|      [[RSA]]       | Yes                   | Yes               | Yes          |
| [[Elliptic Curve]] | Yes                   | Yes               | Yes          |
| [[Diffie-Hellman Key Exchange]] | No                    | No                | Yes          |
|      [[DSS]]       | No                    | Yes               | No           |
## Requirements
[[Asymmetric encryption#Characteristics]]을 만족하기 위해 *trap-door one-way function*을 사용할 수 있다.
### One-way function
$$\begin{align} Y & = f(X) \qquad easy \\ X & = f^{-1}(Y) \quad infeasible\end{align}$$
### Trap-door one-way function
$$\begin{align} Y & = f_k(X) \qquad easy & if \; k \; and \; X \; are \; known\\ X & = f^{-1}_k(Y) \quad\; easy & if \; k \; and \; Y \; are\;known \\ X&=f^{-1}_k(Y) \quad\; infeasible & if \; Y \;known \; but \; k \; not \; known\end{align}$$
## Security
[[Symmetric encryption]]과 동일하게 항상 [[Brute-force attack]]의 가능성은 존재한다. 하지만, 계산이 매우 오래 걸리도록 key의 길이는 항상 매우 크게 사용된다. ($>1024\;bits$)

비대칭 암호에서 중요한 것은 암호화, 복호화는 쉬워야 하며 [[Cryptanalysis]]는 어려워야 하는 것을 둘 다 만족해야 한다는 점이다. 

일반적으로 [[Cryptanalysis]]가 어려워야 하는 것에 초점을 맞추어 매우 큰 길이의 수를 사용한다. 이로 인해 [[Symmetric encryption]]에 비해 느리다는 단점이 존재한다.

## Example
+ [[RSA]]
+ [[Diffie-Hellman Key Exchange]]
+ [[ElGamal Cryptography]]
+ [[ECC(Elliptic Curve Cryptography)]]
+ [[DSS]]