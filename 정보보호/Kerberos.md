사용자가 네트워크 전체에 분산된 서비스에 접근하기를 원하는 개방향 분산 환경에서 서버가 인증된 사용자에 대한 접근을 제한하고, 요청을 인증할 수 있는 시스템이 필요하다. 이 상황은 다음과 같은 위협이 존재할 수 있다. 
+ 유저들이 특정 workstation에 대한 접근을 획득하고, 다른 사용자인 척 할 수 있다. 
+ 유저들이 workstation의 네트워크 주소를 변경할 수 있다. 
+ 유저들이 교환을 도청하고, replay attack을 사용하여 서버에 대한 접근을 얻거나 작업을 중단시킬 수 있다.

이러한 상황을 해결하기 위한 [[인증(Authentication)]] 서비스로, 현재 4, 5 버전이 사용된다.
## Requirements
처음 Kerberos가 제시된 논문에서는 다음과 같은 요구사항을 말하고 있다.
+ Secure(안전성)
+ Reliable(신뢰성)
+ Transparent(투명성)
+ Scalable(확장 가능한)

또한, 이 알고리즘은 [[Needham-Schroeder Protocol]]을 기반으로 구현되었다. 
## Kerberos v4
기본적인 third-party [[인증(Authentication)]] 방법으로, AS(Authentication Server)와 TGS(Ticket Granting Server)를 가지고 실행된다. 이때 [[DES(Data Encryption Standard)]]를 사용한 복잡한 프로토콜을 사용한다. 

아래에서 사용되는 용어의 약자는 다음과 같다.
+ C: client
+ V: server
+ ID: identifier
+ TS: timestamp
+ K: symmetric key
+ AD: network address
### Overview
![[Pasted image 20231213153500.png | 600]]
### AS Exchange to obatin ticket-granting ticket
1. C $\rightarrow$ AS: $ID_C||ID_{tgs}||TS_1$
2. AS $\rightarrow$ C: $E(K_C, [K_{c, tgs} || ID_{tgs} || TS_2 || Lifetime_2 || Ticket_{tgs}])$ $$Ticket_{tgs}=E(K_{tgs}, [K_{c, tgs}||ID_C||AD_C||ID_{tgs} || TS_2 || Lifetime_2])$$
### TGS Exchange to obtain service-granting ticket
1. C $\rightarrow$ TGS: $ID_v||Ticket_{tgs} || Authenticator_c$
2. TGS $\rightarrow$ C: $E(K_{c, tgs},[K_{c, v}||ID_V||TS_4||Ticket_V])$ $$\begin{align} Ticket_{tgs} &= E(K_{tgs}, [K_{c, tgs}||ID_C||AD_C||ID_{tgs} || TS_2 || Lifetime_2]) \\ Ticket_v&=E(K_v, [K_{c, v}||ID_C||AD_C||ID_V||TS_4||Lifetime_4])\\Authenticator_c &=E(K_{c, tgs}, [ID_C||AD_C||TS_3])\end{align}$$
### Cilent/Server Authentication Exchange to obtain service
1. C $\rightarrow$ V: $Ticket_v||Authenticator_c$
2. V $\rightarrow$ C: $E(K_{c, v}, [TS_5 + 1])$ (양방향 인증을 위함) $$\begin{align}Ticket_v&=E(K_v, [K_{c, v}||ID_C||AD_C||ID_V||TS_4||Lifetime_4])\\Authenticator_c &=E(K_{c, v}, [ID_C||AD_C||TS_5])\end{align}$$ 
### Realms
Kerberos 환경은 다음과 같이 구성된다. 
+ Kerberos server
+ Server에 등록된 client의 수
+ Server와 키를 공유하고 있는 application server

이러한 환경을 realm이라는 용어로 사용한다. 한 개의 관리자 도메인을 realm이라고 한다. 여러개의 realm을 가지는 경우 kerberos server는 모두 키를 공유하고 이는 안전해야 한다. 
![[Pasted image 20231213153933.png | 600]]
## Kerberos v5
1990년대 중반에 개발되었다. 인터넷 표준 RFC 1510에 특정되었고, v4에서 향상된 사항은 다음과 같다. 
+ 환경적 단점 해결: encryption algorithm, network protocol, byte order, ticket lifetime, authentication forwarding, interrealm auth
+ 기술적 한계 해결: double encryption, non-standadard mode of use, session keys, password attacks



