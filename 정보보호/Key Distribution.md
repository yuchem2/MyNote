암호학에서 키를 분배하고 관리하는 문제는 복잡하다. 일반적으로 [[Symmetric encryption]]에서는 두 종단이 비밀키를 공유해야 하고, [[Asymmetric Encryption]]에서는 유효한 public key를 가지고 있어야 한다.

## Secret Key Distribution
[[Symmetric encryption]]에서는 두 종단 모두 하나의 secret key를 공유해야 한다. 여기서 중요한 문제는 키를 안전하게 공유하는 방법에 대한 내용이다.  이 문제는 중요한 문제로, 종종 키 분배 과정에서 공격 당해 키에 대한 정보가 노출되는 경우가 발생한다. 

키를 분배할 때 두 종단은 같은 비밀 키를 공유해야 하고, 이 키는 다른 이로부터 보호되어야 한다. 또한 공격자가 키를 알 수 없도록 일정 데이터를 암호화한 후 키 값은 바뀌어야 한다. 
### Using [[Symmetric encryption]]
#### Key Distribution Options
키를 분배하고자 하는 $A, B$가 존재할 때 키 분배는 다음과 같은 방법으로 이뤄질 수 있다.
1. $A$가 물리적으로 $B$에게 비밀 키를 전달
2.  다른 개체 $C$가 키를 결정하고, $A, B$에게 전달
3. $A, B$가 이전 혹은 최근에 사용된 키를 가지고 있다면, 그 키를 암호화해 새로운 키로 사용
4. $A, B$가 각각 다른 개체 $C$와 안전한 통신을 할 수 있다면, C를 통해 $A, B$가 비밀 키 공유
#### Key Hierarchy
Master key를 이용해 session key를 생성함으로써 더 자주 키를 변경할 수 있다. 이를 통해 공격을 방지할 수 있다. 두 계층 말고도 더 계층을 더 나누어 키를 사용할 수 있다. 가장 간단한 방법에서는 2계층으로 나눠 키를 분배한다. 
+ Master key: session key를 암호화하는데 사용한다. 유저에 의해 공유되거나 KDC(Key Distribution Center)에 의해 공유된다.
+ Session key: 일시적인 키로, 통신에서 데이터를 암호화하는데 사용된다. 하나의 논리적인 session을 위한 키이며, 이 session이 종료되면 삭제된다. 
#### Key Distribution Scenario with KDC
![[Pasted image 20231212100929.png | 600]]
위 과정을 통해 봤을 때 KDC를 통한 session key 분배는 다음과 같은 과정으로 이뤄지게 된다. 이때 KDC는 $K_a, K_b$에 대한 정보를 가지고 있음
+ 키 분배 과정
	1. session 생성자 $A$가 $ID_A||ID_B||N_1$을 KDC에게 전송한다.
	2. KDC는 A에게 $E(K_a, [K_s||ID_A||ID_B||N_1])||E(K_b, [K_s||ID_A])$를 전송
	3. A는 자신의 키 $K_a$를 통해 복호화를 수행함으로써 session key $K_s$를 획득한 후 B에게도 session key를 알리고자 $E(K_b, [K_s||ID_a])$를 전송
+ 인증 과정
	1. A는 B에게 session key를 알리고자 B에게 $E(K_b, [K_s||ID_a])$를 전송
	2. B는 복호화를 통해 $K_s$를 획득하고, A에게 $E(K_s, N_2)$를 전송한다
	3. A는 전달받은 값을 복호화해 $N_2$를 획득해 상대방을 확인한다. 확인한 후에 다시 B에게 $E(K_s, f(N_2))$를 전송한다.
	4. B는 이 값을 전달 받고 복호화해 상대방을 확인할 수 있다. 

위에서 사용한 $f$는 각 protocol에서 미리 정한 함수이다. 인증과정에서 사용된 인증 방법은 *challenge-response*라고 하며 인증과정에서 일반적으로 사용되는 방법이다. 
#### Key Distribution Issues
+ 대규모 네트워크에서 KDC가 필요하다. 이때 KDC는 신뢰적인 객체여야 한다. 
+ session key의 수명은 제한되어야 한다.
+ 자동 키 분배를 사용하는 경우 그 시스템이 신뢰적이여야 한다.
+ 분산된 키 분배를 사용해야 한다.
+ 키 사용을 제어할 수 있어야 한다. 
### Using [[Asymmetric Encryption]]
공개키 암호시스템은 비효율적이기 때문에 대규모 블록을 직접 암호화하는데 사용하지 않고, 비교적 작은 블록에 한정되어 사용된다. 즉, 데이터 암호화에 사용되기 보다는 키를 분배할 때 키를 암호화해 전송하는데 사용된다. 
#### Simple Secret Key Distribution
Merkle이 제안한 매우 간단한 방법으로, 암호화 통신을 제공할 수 있다. 통신 이전에는 어떠한 키도 존재하지 않기 때문에 위험을 최소화 할 수 있다. 

![[Pasted image 20231212103039.png|600]]

하지만, 이 방법의 경우 [[Man-In-the-Middle Attack]]에 취약하다. 
![[Pasted image 20231212103404.png | 600]]
#### Secret Key Distribution with Confidentiality and Authentication
다음과 같은 과정으로 수행된다. 
1. A는 B에게 $E(PU_b, [N_1||ID_A])$를 전송한다. 
2. B는 자신의 private key $PR_b$를 통해 복호화해 $N_1$을 획득한 후 A에게 $E(PU_a, [N_1||N_2])$를 전송한다.
3. A는 자신의 private key $PR_a$를 통해 복호화해 $N_2$를 획득하며 $N_1$을 확인함으로써 B가 보낸 것임을 확인한다. 그 후 $E(PU_b, N_2)$를 재전송한다.
4. 이때 A는 B에게 $E(PU_b, E(PR_a, K_s))$도 전송해 session key를 B에게 전달한다.
5. B는 재전송된 $N_2$를 확임함으로써 A를 확인한다. 그 후 $PR_a$와 $PU_a$를 통해 복호화해 session key를 얻게 된다.

![[Pasted image 20231212104000.png | 600]]

이 방법 또한 [[Man-In-the-Middle Attack]]에 취약하다.
#### Hybrid Key Distribution(IBM)
KDC를 사용하는 방법으로, 각 유저는 secret master key를 공유한다. 이때 public key를 사용한다. Master key를 사용해 session key를 분배한다. 

## Public Key Distribution
공개키를 공유하는 방법에는 다양한 방법이 존재한다. 그 중 일반적인 4가지 방법은 다음과 같다. 
+ Public announcement
+ Publicly available directory
+ Public-key authority
+ Public-key certificates
### Public Announcement
각 유저는 자신의 public key를 네트워크 공간에 널리 알린다. 이 방법은 매우 편한 방법이지만, 이로 인해 위조의 문제가 생길 수 있다. Public key에 대한 정보가 모두에게 알려졌기 때문에 누구든 그 키를 생성해내 다른 이에게 보낼 수 있고, 자신이 그 유저라고 주장할 수도 있다. 
### Publicly Available directory
Public key를 공개적으로 이용가능한 동적 디렉토리 안에 유지함으로써 보안성이 높아질 수 있다. 공개 디렉토리의 유지 및 배포는 일부 신뢰된 개체 혹은 조직이 구성해야 한다. 이 내용은 다음과 같은 요소를 포함한다. 
+ {name, pulbic-key} 정보를 함께 유지
+ 각 정보 제공자는 디렉토리 권한에 public key를 등록
+ 각 정보 제공자는 언제나 키를 바꿀 수 있다.
+ 디렉토리는 주기적으로 public key를 공개해야 한다.
+ 디렉토리는 전자적으로 접근 가능해야 한다.

이 방법도 여전히 위조와 변경에 대한 문제가 존재한다. 
### Public-key authority
Publicly avaialbe directory 방법에서 제어를 단단히 함으로써 보안성을 향상시킨 방법으로, 각 유저는 public key를 얻기 위해 directory와 상호작용을 실시해야 한다. 이로 인해 실시간 접근이 허용되어야 한다. 이 상호작용의 시나리오는 다음 그림과 같다. 

![[Pasted image 20231212110338.png | 600]]
### Public-key certificates
현재 사용되는 방법으로, public-key authority 방법에서 실시간 접근을 사용하지 않는 방법이다. 사용자의 정보와 public key를 통해 인증을 발급하여 진행하게 된다. 실시간 접근을 허용하는 경우 인증기관의 부하가 증가하게 되는데, 이를 감소시키기 위해 실시간 접근이 아니라 유효기간 동안 한번만 접근을 허용해 public key로서 사용될 수 있는 인증서를 발급한다. 여기서 발급된 인증서를 자신의 public-key처럼 사용한다. 

![[Pasted image 20231212110501.png | 600]]

### X.509 Authentication Service
CCITT X.500 dericeroy service 표준의 일부분이다. 여기서 directory는 유저에 대한 정보 데이터베이스를 유지하는 분산 서버를 말한다. 이 표준에서는 인증 서비스를 위한 framework를 정의하고 있다(Directory는 certication authority에 의해 인증된 유저의 public key 정보를 저장하고 있다). 또한, 인증 protocol을 정의하고 있다. 표준에서는 public-key crpyto와 [[Digital Signature]]를 사용할 수 있다고 정의하고 있다. 이때 [[Digital Signature]]의 경우 알고리즘은 표준을 정의하고 있지 않지만 [[RSA]]를 추천하고 있다. 

![[Pasted image 20231212112952.png | 600]]
#### Certificates
X.509 방법의 핵심은 public-key certificate이다. 이 값은 CA에 의해 생성되며 다음과 같은 정보를 포함한다. 
+ version V(1, 2, or 3)
+ serial number SN(unique with CA) identifying certificate
+ signature algorithm identifier AI
+ issuer X.500 name CA
+ period of valditiy TA(from - to dates)
+ subject X.500 name A(name of owner)
+ subject public-key info $A_p$ (algoirithm, parameters, key)
+ issuer unique identifier(v2+)
+ subjct unique identifier(v2+)
+ extension fields(v3)
+ signature(of hash of all fields in certificate)

![[Pasted image 20231212113501.png | 600]]

또한, $CA<<A>>$로 표기할 수 있으며 이는 CA에 의해 서명된 A의 인증이라는 의미이다. 유저가 CA에 접속하면 인증을 얻을 수 잇다. 오직 CA만이 이 값을 수정할 수 있다. 이러한 이유는 위조를 방지하기 위함이다. 인증들은 public derectory에 저장되어 유지된다. 
#### Certificate Revocation
모든 인증은 유효기간이 존재한다. 이 때 인증이 만료되기 전에 취소하는 경우가 발생할 수 있다. e.g. 유저의 private key가 손상, 유저가 해당 CA의 인증을 더 이상 사용하지 않음, CA의 인증이 손상

각 CA는 취소된 인증에 대한 정보를 CRL(Certificate Revocation List)에 저장해 유지한다. 이를 통해 취소된 인증을 사용하는 것을 방지할 수 있고, 유저들은 자신이 받은 인증이 CRL에 있는 지 확인해야 한다. 
#### CA Hierarchy
통신하고자 하는 두 유저가 같은 CA를 공유한다면 그들은 서로의 public key를 알 수 있다. 그러나, CA는 계층화되어 있기 때문에 두 유저가 같은 CA를 공유하고 있지 않을 수 있다. 이때 각 유저가 통신하기 위해서는 자신의 인증을 계층을 통해 변화시켜 사용해야 한다. 

![[Pasted image 20231212114131.png | 600]]

위 그림에서 A가 얻는 B의 인증은 다음과 같다. $$X<<W>> W<<V>> V<<Y>> Y <<Z>> Z <<B>>$$위 값을 차례대로 풀면서 B의 public key를 얻을 수 있다. B가 A로 메시지를 보내기 위해서는 B도 A의 인증을 확인해 public key를 얻을 수 있다. $$Z<<Y>>Y<<V>>V<<W>>W<<X>>X<<A>>$$
### PKI(Public Key Infrastructure)
![[Pasted image 20231212115613.png | 600]]
