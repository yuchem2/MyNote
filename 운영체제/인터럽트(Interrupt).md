
이벤트 발생은 일반적으로 HW, SW의 [[인터럽트(Interrupt)]]에 의해 알려진다. 결국 "상태의 변화"를 제어측(CPU)에 알려주는 방법이다. 즉, 인터럽트는 HW가 발생시킨 것과 SW가 발생시킨 것으로 나눌 수 있다.

+ HW interrupt: 항상 CPU에 신호를 보냄으로써 발생시킬 수 있다.  ([[Device controller]], [[Timer]])
+ SW interrupt: system call에 의한 특정 작업을 수행함으로써 발생시킬 수 있다. [[트랩(Trap)]]이라고도 하며 에러나 사용자 요구에 의해 발생한다

## **Interrupt가 발생하면 CPU는??**
---
1. 진행중인 작업을 중지한다
2. 즉시 fixed location으로 실행을 옮긴다. Fixed location은 해당하는 [[Interrupt Service Routine]]가 위치한 시작 주소를 말한다. 
3. [[Interrupt Service Routine]]을 수행한다
4. Interrupt를 위한 작업이 종료되면 다시 이전에 수행하던 작업을 재개한다



## **Interrupt Common Functions**
---
+ 일반적으로 [[Interrupt Vector]]을 통해 [[Interrupt Service Routine]]에게 제어를 운반한다. 
+ Interrupt instruction의 주소를 저장해야 한다. ([[PC(Program Counter)]]와 [[레지스터(Register)]])
+ 이미 처리 중인 Interrupt가 존재하면 새로운 Interrupt는 손실을 막기 위해 실행되지 않는다

※ [[운영체제(Operating System)]]는 Interrupt를 처리하며 기반으로 동작한다.