> This term refers to protection of networks and their service from unauthorized modification, destruction, or disclosure, and provision of assurance that the network performs its critical functions correctly and there are no harmful side effects.

네트워크와 그 서비스를 무단으로 변경, 파괴,  공개로부터 보호하고 네트워크가 중요한 기능을 올바르게 수행하고 유해한 부작용이 없음을 보장한다. 

크게 communication과 device에 대한 보안으로 나누어 살펴볼 수 있다

Communication 보안은 네트워크 통신을 할 때 사용되는 context을 보호하는 매커니즘에 대한 내용이며 [[Passive attack]]과 [[Active attack]]을 감지하고, 방어하는 것이 포함된다. 
+ Network Protocols
	+ [[IPsec]]
	+ [[TLS]]
	+ [[HTTPS]]
	+ [[SSH]]
	+ [[IEEE 802.11i]]
	+ [[S//MIME]]
	+ etc...
+ [[암호화 알고리즘(Cypotographic algorithms)]]

Device 보안은 네트워크 장비에대한 보안을 말한다. 
+ [[Firewall]]
+ [[Intrusion detection]]
+ [[Intrusion prevention]]

## Model for network security
---
+ Plain text  $\rightarrow$ Cyper text $\rightarrow$ Plain text
+ 일반적으로 [[Trust Model]] 개념을 이용해 암호화, 복호화를 진행하며 네트워크 상에는 암호문만 존재하게 된다
+ Network security 모델을 사용하기 위해서는 다음과 같은 요소들이 필요하다
	+ 적절한 알고리즘을 설계
	+ 알고리즘에서 사용되는 암호키를 구성
	+ 암호키를 분산시키거나 공유하는 방법의 개발
	+ 정보와 암호키를 사용하는 원칙에 대한 protocol을 특정화
+ 이 모델은 크게 다음과 같은 요소들로 정의할 수 있다
	+ Opponent: 유저나 유저 소프트웨어를 말함
	+ Access channel: 접근 가능한 경로
	+ Gatekeeper function: 인증되거나 허용된 접근만 Information system에 접근 할 수 있도록 제한하는 함수
	+ Information System: 다양한 정보, 리소스들이 존재하는 공간
+ 위 요소들에 따라 Gatekeeper function이 각 유저를 구별하는 방법을 잘 정의할 필요가 있고 보안 제어를 어떻게 구성하는지가 중요하다
