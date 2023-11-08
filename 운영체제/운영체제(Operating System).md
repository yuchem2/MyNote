
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

## **Services**
---
사용자에게 도움이 되는 기능들을 제공
+ [[UI(User Operating System Interface)]]
+ Program Execution: 프로그램을 메모리에 적재하고, 적재한 프로그램을 실행하고, 정상적이든 비정상적(에러를 포함)이든 종료할 수 있어야함
+ [[입출력]] 연산: 실행 프로그램은 파일이나 입출력 장치를 포함하는 입출력 요청 가능
+ [[File-System Manipulation]]
+ Communications: 같은 컴퓨터 내 혹은 네트워크를 통해 다른 컴퓨터와 정보 교환 가능
                                공유 메모리나 메시지 전송을 통해 가능
+ Error Detection: 운영체제는 끊임없이 가능한 오류에 대해 인식할 필요가 있고, 오류 종류에 따라 적절한 행동을 취해야 함
+ Resource allocation: 여러 사용자와 여러 작업이 동시에 수행될 때 OS가 적절하게 자원을 할당하고 분배할 필요가 있음
+ 계정: 어떤 사용자가 어떤 종류의 컴퓨터 자원을 얼마나 사용했는지 기록(keep track of)
+ [[Proctection]] and [[Security]]: 정보 소유자는 정보의 사용을 제어하고, 동시에 수행되는 프로세스 끼리 서로 간섭하지 않도록 함
+ [[System Call]]
+ [[System Program]]


## **Design and Implementation**
---
아직 완전한 해결책은 없고, 성공적인 접근 방법들이 존재한다

목적과 특징에 따라 설계와 구현이 달라지며 이에 대해 잘 정의할 필요가 있다.
+ Batch / Multiprogramming / Multitasking
+ Single-user / Multi-user
+ Distributed / Read-time / General purpose

*User Goal: 사용하기 편리하고, 배우기 쉽고, 믿을 수 있고, 안전하고, 신속해야함
System Goal: 설계, 구현, 유지보수 용이, 적응성, 신뢰성, 무오류, 효율성*

Policy와 mechanism 사이를 잘 조정할 필요가 있다
+ Mechnism: How?
+ Policy: what

전통적으로 [[어셈블리 언어(Assembly Language)]]로 구현되었지만, 현재는 [[고급 프로그래밍 언어]]로 구현되고 있다

## **Structure**
---
+ [[Simple Structure]]
+ [[Layered Structure]]
+ [[Microkernel System Structure]]
+ [[Modular Kernel Structure]]: Modern OS