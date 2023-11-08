실행 중인 프로그램을 일컫는다. 즉, 시스템 내의 작업의 단위로서 사용된다

작업을 수행하기 위해 다음과 같은 리소스를 필요로 한다
+ CPU, [[Main Memory]], [[입출력]], 파일
+ 초기화 데이터


리소스는 [[운영체제(Operating System)]]에게 [[System call]]을 통해 요구해 해당 리소스를 전달받는다

## [[Thread]] 수에 따라 구분
+ [[Single-threaded process]]
+ [[Mulit-threaded process]]

## 실행 간 특징 구분
+ I/O-bound process: [[입출력]]에 계산보다 더 많은 시간을 소비(많고, 짧은 CPU burst)
+ CPU-bound process: 계산하는데 많은 시간을 소비(소수의 긴 CPU burst)

## 프로세스간 협력
어떠한 이유에 의해 프로세스 간의 협력을 수행할 수도 있다
+ Independent: 다른 프로세스의 실행에 영향을 끼치지도 않고, 받지도 않음
+ Cooperating: 다른 프로세스의 실행에 영향을 끼칠 수 있고, 받을 수 도 있음

다음과 같은 장점이 있으며 소통하는 방법을 [[IPC (InterProcess Communication)]]라고 한다
+ 정보 공유
+ 계산 속도의 향상(병렬성)
+ 모듈화(기능에 따라 분리된 프로세스 제작)
+ 편리성(동시에 수행되는 환경)

## **Process in Memory**
---
+ 프로세스의 현재 활동 정보는 [[PC(Program Counter)]]와 processor의 [[레지스터(Register)]]의 내용에 의해 나타난다
+ [[스택(Stack)]]: 일시적 자료를 포함한다(함수 매개변수, 반환 주소, 지역 변수들...)
+ [[Data section]]: 전역 변수들을 포함한다
+ [[힙(Heap)]]: 실행 시간 동안 동적으로 할당되는 메모리
+ Text: program code가 저장되어 있는 영역

기본적으로는 독립적으로 메모리 영역이 생기지만, 같은 프로그램 내에서 연관될 수도 있다
## **Process State**
---
+ new: 프로세스가 생성(메모리에 프로그램이 올라감)
+ running: 실제 코드가 실행 중
+ waiting: 이벤트 발생을 기다리는 상태
+ ready: CPU가 할당되기를 기다리는 상태
+ terminated: 실행이 종료된 상태
오직 한 개의 process만이 running state에 존재할 수 있고, 대부분의 state는 ready나 waiting에 존재

새로운 프로세스가 ready queue에 들어오게 되면 그 process는 dispatched될 때가지 기다린다

한 번 process에 CPU가 할당되면 아래 사건들 중 하나가 발생할 때까지 실행된다
+ I/O 요청을 하는 경우
+ 새로운 child process를 생성하는 경우
+ [[인터럽트(Interrupt)]] ([[Timer]] event)를 기다리는 경우
+ 할당된 시간이 만료된 경우
## **Process Operations**
---
+ [[Process Creation]]
+ [[Process Termination]]

## **Process Execution time**
---
프로세스의 실행 시간은 CPU execution(CPU burst)와 I/O wait(I/O burst)의 사이클로 구성된다. 

일반적으로 CPU burst 분산은 exponential or hyper-exponential하게 나타난다. 

많고 짧은 CPU bursts로 구성된 process를 I/O bounded process, 적고 긴 CPU bursts로 구성된 process를 CPU bounded process라고 한다. 