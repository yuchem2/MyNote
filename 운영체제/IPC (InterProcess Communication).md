
[[운영체제/Process]]간의 Commniucate와 Synchronize에 관련된 mechanims

## 공유 메모리
각 프로세스들 간에 공유하는 메모리 영역을 만들어 사용(Directly)

대용량 작업에 유리하고, 소통하는데 편리함이 있으며 메시지 전달 방식에 비해 빠르다

공유 메모리 영역을 만들 때만 [[System Call]]을 필요로 함

이러한 방식을 채택한 system을 [[Shared-Memory System]]이라고 한다

## 메세지 전달
각 프로세스들 간에 message를 서로 보내 데이터를 공유([[운영체제(Operating System)]]가 관여)

쉽게 구현할 수 있고, 적은 양의 데이터가 이동할 때 유용하다

메시지를 이용할 때마다 [[System Call]]을 필요로 한다

이러한 방식을 채택한 system을 [[Message-Passing System]]이라고 한다


추가적으로 [[Client-Sever Communication]]이 존재한다