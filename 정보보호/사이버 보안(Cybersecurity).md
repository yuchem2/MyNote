> Cybersecurity is the protection of information that is stored, transmitted, and processed in a networked system of computers, other digital devices, and network devices and transmission lines, including the Internet. Protection encompasses confidentiality, integrity, availability, authenticity, and accountability. Methods of protection include organizational policies and procedures, as well as technical means such as encryption and secure communications protocols. 


사이버 보안은 저장되거나 전송되는 정보를 보호하는 것을 말한다. 이때 정보는 네트워크 상의 컴퓨터나 다른 디지털 장비 사이에서 전송될 수 있고, 네트워크 장비와 전송 라인 사이에서 있을 수도 있으며 인터넷 상에 존재할 수 있다. 

보호에는 기밀성, 무결성, 가용성, 진정성 및 책임이 포함된다. 보호 방법에는 조직 내 정책과 절차, 암호화 및 통신 프로토콜과 같은 기술적 수단이 포함된다. 

사이버 보안은 [[정보 보호(Information Security)]]과 [[네트워크 보안(Network security)]]으로 나눌 수 있다. 


## **Security Objectives**
---
+ *기밀성(Confidentiality)*
	+ [[데이터 기밀성(Data Confidentiality)]]: 개인 또는 기밀 정보가 허가 받지 않은 개인이 이용할 수 없도록 하거나 공개하지 않는 것을 보장한다
	+ Privacy: 개인이 자신과 관련된 정보를 수집하고 저장할 수 있으며 누구에게 정보를 공개할 수 있는지를 통제하거나 영향력을 행사할 수 있도록 보장한다
+ *무결성(Integrity)*
	+ [[데이터 무결성(Data Integrity)]]: 데이터와 프로그램이 모두 특정하게 승인된 방식으로만 변경되도록 보장. 추가적으로 data authenticity를 포함한다. 
	+ System integrity: 시스템이 정상적인 접근으로는 의도된 방식으로 수행되고, 고의적이거나 부주의한 무단 조작에 자유로운 것을 보장
+ *사용가능성(Availability)*: 시스템이 신속하게 작동하고, 승인된 사용자에게 서비스를 거부하지 않음을 보장

위 세 요소는 종종 *CIA triad* 라고 불린다. 

## **Computer security Challenges**
---
1. 요구사항은 간단하지만, 이를 충족하기 위한 메커니즘은 상당히 복잡할 수 있다
2. 잠재적인 공격을 고려해야만 한다
3. 특정 절차는 종종 직관과 다를 수 있다 
4. 알고리즘과 비밀 정보를 포함한다
5. 어디에 메커니즘을 배치할지 결정해야 한다
6. 공격자와 관리자 간의 눈치 싸움이다
7. 보안 실패가 발생하기 전까지 투자의 해택이 거의 없다는 인식
8. 정규적이고, 지속적인 모니터링이 필요하다
9. 일반적으로 자주 설계 마지막 단계에서 고려된다
10. 시스템 사용의 장애로 간주되기도 한다


OSI 표준으로는 ITU-T(국제전기통신표준화기구)에서 정한 X.800 Security Architecture for OSI가 있다. 이러한 표준은 보안 요구사항을 정의/해석하는 체계적인 방법을 정의하고 있고, 우리가 공부할 개념의 유용한 개요를 추상적으로 제공한다.

## **Aspects of Security**
---
+ [[Security attack]]: 소유한 정보의 보안을 손상시키는 모든 행위![[Security attack]]
+ [[Security mechanism]]: 보안 공격을 탐지, 예방 또는 복구하도록 설계된 프로세스
+ [[Security service]]: 하나 이상의 보안 메커니즘을 이용하여 서비스를 제공하는 서비스![[Security service]]
## **종류**
---
+ [[정보 보호(Information Security)]]
+ [[네트워크 보안(Network security)]]
+ Unconditional security: 컴퓨터 파워가 매우 강력해도 암호문에서 평문을 예측할 수 없는 보안 정책. 사실상 불가능에 가까운 이상적인 형태의 보안
+ Computational security: 컴퓨터 리소르를 제한적으로 생각할 대 암호문에서 평문을 예측할 수 없는 보안 정책. 현실적인 방법으로 언젠가는 암호에 대한 내용이 풀릴 수 있다.