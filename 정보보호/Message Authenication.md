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
+ Disclosure
+ Traffic analysis
+ Masquerade
+ Content modification
+ Sequence modification
+ Timing modification
+ Source repudiation
+ Destination repuditaion