
kernel을 가능한 한 최대로 경량화한 [[운영체제(Operating System)]]

많은 기능들을 user space로 이동시켰다. 통신이 사용자 모듈 사이의 메시지 전달을 통해 구성

Carnegie Mellon Univ에서 Mach OS를 구현하며 처음 제안된 형태이다

## 장점
+ Microkernel을 확장하기 쉬움
+ [[운영체제(Operating System)]]를 새로운 구조로 이식하기 쉬움
+ Kernel에서 수행되는 코드가 적어져 더 안정적이고 안전함


## 단점
+ Kernel과 통신하기 위해 User 공간에 performance overhead가 생김
