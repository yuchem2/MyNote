
[[운영체제(Operating System)]]을 여러 계층으로 구분하여 구성
+ leyer 0: HW
+ leyer N: [[UI(User Operating System Interface)]]

*모듈화*를 통해 각 계층은 바로 아래의 계층에서 제공하는 기능과 서비스만을 사용
+ 전형적인 layer M은 데이터 구조와 상위 계층에 의해 호출될 수 있는 작업 집합으로 구성
+ layer M은 하위 계층에 대한 연산을 호출할 수 있음

## 장점
+ 간단한 구조이며 디버깅이 쉬움
+ 설계와 구현이 단순
+ 각 계층은 하위 계층에서 제공하는 연산만을 통해 구현. 이 연산이 어떻게 구현되었는지는 알 필요가 없음

## 단점
+ 계층의 확장, 추가가 어려움
+ 계층을 한번 만들 때 잘 구성해서 제작할 필요가 있음


## UNIX
---
System programs와 kernel로 구분되어 구성
+ kernel layer가 system call interface부터 물리적인 하드웨워까지 모두 포함 -> 하나의 계층에서 너무 많은 기능들을 수행
+ kernel layer가 다른 레이어에 비해 매우 큼