User, user를 대신하여 작동하는 appilcation/process가 실제로 누구인지 또는 무엇인지 결정하는 과정을 말한다. [[인증(Authentication)]] 기술은 현재 유저가 인증된 유저임을 검증함으로써 시스템의 제어권을 제공한다. 

이러한 특징으로 인해 보안성 유지에 기초가 되는 기술로 볼 수 있다. 

인증 과정은 두 개의 단계로 구분할 수 있다. 
1. Identification: 특정 객체를 식별할 수 있도록 인증서 등으로 특정한다. 
2. Verification: 특정 객체나 사람 등을 식별해 검증한다. 

이 과정은 [[Message Authenication]]과 구분되어 별개의 동작으로 수행된다. 
## Means
Authentication factors라고도 하는 유저의 identity를 인증하는 요소들은 다음과 같다.
+ Knowledge factor: 유저에 의해 결정되는 비밀 정보로, single-layer authentication 과정에서 사용된다. e.g. password, PINs(personal identification numbers)
+ Possession factor: 유저가 client computer나 portal을 통해 연결된 물리적인 entity를 말한다. 이러한 형태의 정보는 token이라고도 한다. 현재는 physical token이라고 하며 다음과 같이 구분한다. 
	+ Connected H/W tokens: 논리적 혹은 물리적으로 컴퓨터와 연결된 객체 중 identity를 인증할 수 있는 항목. e.g. smart cards, wireless tags, USB 
	+ Disconnected H/W tokens: 직접적으로 client computer에 연결되지 않고, 로그인을 시도하는 개인의 입력을 요구하는 항목. 내장된 스크린을 사용하여 인증 데이터를 보여주고, 사용자에 의해 입력될 때 사용된다.  e.g. key, token
+ Inherence factor(is/does): biometrics(생물 통계학)이라고 불리는 특성을 사용하는 것으로, 개인마다 완벽하게 유일하거나 거의 유일한 정보를 이용한다. 
	+ Static: fingerprint, retina, face, etc.
	+ Dynamic: voice, handwriting, sign, typing rhythm, etc.

![[Pasted image 20231213102429.png | 600]]

이러한 정보들은 각각 사용되어 인증되거나 혼용해 같이 인증 정보로 사용될 수 있다. 
## Authentication Protocols
### Mutual Authentication
두 당사자가 동시에 서로를 인증하는 프로토콜. 데이터 보안을 보장하기 위해 민감한 데이터를 전송하는 시스템에서 원하는 특성이다. 일반적으로 유저 이름과 비밀번호, 공개 키 인증서를 authentication factor로 사용할 수 있다.

Replay Attacks에 의해 공격 받을 위험성이 존재한다. 해당 프로토콜을 공격하는 몇 가지 예시는 다음과 같다.
+ Simple replay: 메시지를 복사한 뒤 다시 보냄
+ Repetition that can be logged: 공격자가 시간이 기록된 메시지를 유효한 시간안에 재전송.
+ Repetition that cannot be detected: 공격자가 시간이 기록된 메시지를 유효한 시간안에 재전송. 추가적으로 원본 메시지를 은폐시킨다. 
+ Backward replay without modification: 수정없이 재전송. [[Symmetric encryption]]을 사용하여 메시지를 기준으로 발신, 수신 메시지의 차이를 인식할 수 없는 경우에 가능

공격에 대응하는 방법들은 다음과 같다.
+ 순서 번호(Sequence number)를 사용한다.
+ Timestamps: 시간 정보를 메시지에 포함 시킴. 이때 두 당사자의 clock을 동기화 해야 함
+ Challenge/Response: 통신 시작 시에 Nonce 값을 보냄으로써 그에 대응하는 응답을 받는 것을 통해 대응
### One-way Authentication
두 당사자 중 한 쪽만 인증하는 프로토콜. 두 당사자가 동시에 통신하지 않는 시스템에서 사용하는 특성이다. (e.g. email) 

## Using [[Symmetric encryption]]
[[Key Distribution#Using Symmetric encryption]]처럼 master-session 두 키를 사용한다. 이때 신뢰적인 KDC를 사용하는 방법이 일반적으로 사용된다. 
### [[Needham-Schroeder Protocol]]
![[Needham-Schroeder Protocol]]
### One-Way Authentication
KDC를 개량해 사용하는 방법으로 앞서 언급한 것처럼 email 등에서 사용된다. [[Needham-Schroeder Protocol]] 과정에서 4, 5번을 제외하고 사용한다면 다음과 같은 과정을 거친다. 
1. A $\rightarrow$ KDC: $ID_A || ID_B || N_1$
2. KDC $\rightarrow$ A: $E(K_a, [K_s || ID_B || N_1 || E(K_b, [K_s || ID_A])])$
3. A $\rightarrow$ B: $E(K_b, [K_s || ID_A])||E(K_s, M)$

이 경우 또한 replay attack에 의해 보호되지 않는다. 
### [[Kerberos]]
![[Kerberos]]
## Using [[Asymmetric Encryption]]
### Mutual Authentication
![[Key Distribution#Public-key authority]]
공개 키를 공유하기 위해 [[Key Distribution]]에서 논의한 공유 키 방법을 사용한다. 이때 다양한 공격의 위험을 제거하기 위해 다음과 같은 변경사항이 제시되었다.
+ Denning Protocol using timestamps
+ Woo & Lam protocol using nonces
### One-way Authentication
email 등에서 사용하기 위한 방법이다. 일반적으로 긴 메시지가 전송되기 때문에 public key alogrithm 부분에 비용이 많이 든다. 

보안성을 위해 one-time secret key와 public key를 사용해 암호화 한다. 인증을 위해 [[Digital Signature]]를 사용한다. 

## Federated Identity Management
연합 신원 관리는 여러 기업과 수 많은 application에 걸쳐 공통 신원 관리 체계를 사용하고 수천 명, 심지어 수백만 명의 사용자를 지원하는 새로운 개념. 

![[Pasted image 20231213155747.png | 600]]
