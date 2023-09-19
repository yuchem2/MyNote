
> A program that acts as an intermediary between a user of a computer and the computer hardware.


## **Objective**
---
+ [[Process Management]]: User Program을 실행하고, 편리하게 사용할 수 있는 환경 제공
+ Resource Management: Hardware를 효율적으로 관리
	+ [[Memory Management]]
	+ [[Storage Management]]
	+ [[입출력]], [[입출력 서브시스템]]
+ [[Environment Management]]: System을 효율적으로 사용할 수 있는 환경 제공


## **Definition**
---
> OS is a resource allocator and a control program. 

+ 효율적이고, 공정한 자원의 사용을 위하여 충돌하는 요구들을 결정하는 역할 수행
+ 잘못된 사용과 에러를 방지하기 위해 프로그램의 실행을 제어


## **Structure**
---
+ [[Multi-programming]]: 호율성을 위해 필요. process의 목적 달성이 중요함
+ Timesharing([[Multitasking]]): 마치 각 작업이 동시에 수행되는 것처럼 사용자가 느낄 수 있도록 함


## **Operations**
---
+ 현대 OS는 HW에 의한 [[인터럽트(Interrupt)]] driven 방식으로 구동한다
+ SW 오류나 요청은 예외나 [[트랩(Trap)]]트랩을 생성한다
+ [[Dual-mode opertaion]]은 OS가 다른 시스템 구성요소와 OS를 스스로 보호하는 것을 가능케 한다
	+ User mode
	+ Kernel mode
+ [[Process Management]]
+ Resource Management
	+ [[Memory Management]]
	+ [[Storage Management]]
	+ [[입출력]], [[입출력 서브시스템]]
+ [[Environment Management]]
+ [[Proctection]] and [[Security]]