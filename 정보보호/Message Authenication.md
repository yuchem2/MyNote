수신자가 받은 메시지가 자신이 원하던 메시지임을 인증하기 위한 프로토콜로 다음과 같은 특징을 가진다. 
+ 메시지 무결성 유지
+ 메시지 송신자를 검증
+ [[부인방지(Non-repudiation)]]
![[부인방지(Non-repudiation)]]

구현에 다음과 같은 방법들이 사용될 수 있다. 
+ [[Cryptographic Hash Function]]
+ 메시지 암호화
+ [[MAC(Message Authentication Code)]]
## Requirements
+ Disclosure(공개): 적절한 암호키를 소유하지 않은 모든 사용자에게 메시지 내용 공개
+ Traffic analysis(트래픽 분석): 통신에서의 트래픽 패턴을 발견한다. 연결 지향적인 프로그램에서 통신의 빈도 및 지속시간이 결정될 수 있다. 또한, 메시지의 길이나 수가 결정될 수 있다.
+ Masquerade(가장, 속이는): 부정한 소스로부터 네트워크에 메세지를 삽입하는 것. 인증된 종단으로 오는 것으로 알려진 메시지가 상대방에 의해 작성된 것을 포함한다. 또한 메시지 수신자 외에 다른 사람에 의한 메시지 수신 또는 비수신에 대한 부정한 인정도 포함
+ Content modification(내용 수정): 삽입, 삭제, 전치 및 수정을 포함해 메시지 내용을 변경
+ Sequence modification(순열 수정): 메시지 순열에 대해 삽입, 삭제, 순서 바꾸기 등의 변경
+ Timing modification(시기 수정): 메시지에 딜레이를 추가하거나 재전송. 
+ Source repudiation(출발지 거부): 출처별 메시지 전송 거부
+ Destination repuditaion(목적지 거부): 대상별 메시지 수신 거부
## Message Encryption
### Symmetric Message Encryption
### Public-Key Message Encryption
