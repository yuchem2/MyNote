
가역적인 알고리즘과 비가역적인 알고리즘으로 구분할 수 있다

가역적인 암호화 알고리즘은 단순히 데이터가 암호된 후에 복호화되도록 허용하는 알고리즘으로 암호 Key가 존재한다(Single-key, Two-key)

비가역적인 암호화 알고리즘은 암호 Key가 따로 존재하지 않는다(Keyless). [[Digital Signature]]과 [[Message Authenication]]에서 사용된다.
## **키를 통한 분류**
### Keyless
결정론적인 함수들(Deterministic functions)을 사용함으로써 암호화하는 알고리즘
+ [[Cryptographic Hash Function]]
+ [[PRNG(Pseudo-Random Number Generator)]]
### Single-Key
하나의 사용자 비밀 키를 이용한 알고리즘
+ [[Symmetric encryption]]
	+ [[Block Cipher Symmetric Encryption]]
	+ [[Stream Cipher Symmetric Encryption]]
+ [[MAC(Message Authentication Code)]]

### Two-key
두 개의 사용자 비밀 키를 이용한 알고리즘. Private key와 public key를 사용한다.
+ [[Asymmetric Encryption]]
	+ [[Block Cipher Asymmetric Encryption]]
	+ [[Stream Cipher Asymmetric Encryption]]
+ [[Digital Signature]]
+ [[Key Distribution]]
+ [[Message Authenication]]
+ [[User Authentication]]
### [[Symmetric encryption]] vs [[Asymmetric Encryption]]

|                   | Conventional Encryption                                                                                                                                              | Public-Key Encrpytion                                                                                   |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| Need to Work      | 암호화와 복호화에 같은 키, 같은 알고리즘을 적용. 송$\cdot$수신자는 알고리즘과 키를 공유해야 한다.                                                                    | 각 키에 하나의 알고리즘이 존재하고, 각각 암호화, 복호화에 사용된다. 송$\cdot$수신자는 키 쌍을 공유한다. |
| Need for security | 키는 보호되어야 한다. 다른 이가 정보를 절대 해석할 수 없거나 최소 복호화를 못해야 한다. 알고리즘을 알고, 암호문 예시를 가지고 있다고 해도 키를 결정할 수 없어야 한다. | 두 개 키 중 하나는 보호되어야 한다. 다른 이가 정보를 절대 해석할 수 없거나 최소 복호화를 못해야 한다. 알고리즘을 알고, 암호문 예시를 가지고, 공개 키를 알고 있다고 해도 비밀 키를 결정할 수 없어야 한다.                                                                                                          |

## Scheme
암호화 알고리즘은 절대적으로 안전해야 하지만, 사용자가 많다면 필연적으로 평문과 암호문에 대한 데이터 양이 많아질 수 밖에 없고 특정 유저가 이를 이용해 예측할 수 도 있다. 

그러므로 모든 암호화 알고리즘은 그것이 안전하다는 것에 대해 시간적 제한이 존재한다고 볼 수 있다. 즉, 절대적으로 안전한 암호화 알고리즘은 존재하지 않는다.

그러므로 암호화 알고리즘을 사용하는 유저들은 다음 두 개의 특성 중 하나 이상을 만족하는 알고리즘을 추구할 수 있다
+ 암호를 푸는데 드는 비용이 암호화된 정보의 가치를 초과한다
+ 암호를 해독하는데 필요한 시간이 유효 수명을 초과한다

위와 같은 내용을 Encrpytion sheme이라고 말하며 이것은 실질적인 보안 정책을 말한다