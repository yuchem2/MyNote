CPU를 여러 [[운영체제/Process]] 중 어떤 process에 할당할지 결정하는 작업, 즉 ready queue에 있는 [[운영체제/Process]]를 선택해 CPU를 할당하는 작업

CPU scheduling, Process scheduling, Kernel Tread scheduing 세 용어가 같은 의미로 사용된다.

각 목적에 따라 어떻게 실행시킬지 결정한다
+ [[Multi-programming]]의 목적은 CPU utilization을 최대화하는 것임으로, 항상 process들을 실행시키고자 한다
+ [[Multitasking]]의 목적은 실제로 동시에 실행하는 것처럼 보이기 위함으로, CPU를 process간 계속 switch한다

[[큐(Queue)]]를 기반으로 작동하며 다음과 같은 큐들을 가지고 있다.
+ Job queue: 시스템 내 모든 프로세스 집합
+ Ready queue: ready state인 process들의 집합
+ Device queue: wait state인 process들 중 I/O를 기다리고 있는 process들의 집합
	+ 각 Device들은 고유한 자신의 queue를 가지고 있음
## [[Process Scheduler]]
[[운영체제(Operating System)]] module 중 하나로, 메모리에 있는 ready 상태의 프로세스 중 하나를 선택하고, CPU를 할당하는 일을 수행한다. 
![[Process Scheduler]]

다음과 같은 상황에서 활성화되어 ready queue에서 process를 선택한다
1. 한 프로세스가 running state에서 waiting state로 switch: I/O request, invocation of wait(), sleep(), fork(), pause()
2. 한 프로세스가 running state에서 ready state로 switch: [[인터럽트(Interrupt)]] 발생 시
3. 한 프로세스가 waiting state에서 ready state로 switch: I/O 종료 시
4. 한 프로세스가 종료(running state에서 terminated state로)
5. 한 프로세스가 시작(new state에서 ready state로)

위 경우에서 1, 4의 경우를 non-preemptive scheduling이라고 하고, 그 외의 경우를 preemptive scheduling이라고 한다.
### Non-preemptive
+ 프로세스가 CPU를 한번 할당받으면, 프로세스는 종료되거나 waiting state로 switch될 때까지 CPU를 놓지 않는다.
+ Windows 3.x 에서 사용되었다.
### Preemptive
+ 현재 실행되고 있는 프로세스가 언제나 다른 프로세스에 의해 swtich될 수 있음
+ 현대의 대부분의 OS가 이와 같은 방식을 제공한다. (Windows, Mac OS, UNIX, ...)
+ 프로세스들 간의 공유 데이터에 접근하는 것과 관련된 비용이 발생할 수 있다
	+ 특정 OS(UNIX)는 [[Context Switch]]를 하기 전에 I/O block이 수행되고 있으면, 그 작업이 종료될 때까지 기다린다. 
### Dispatcher
+ process scheduler에 속하는 부분
+ [[Sort-term Scheduler(CPU Scheduler)]]에 의해 선택된 프로세스에게 CPU의 제어권을 주는 역할을 수행. 다음과 같은 경우에 실행될 수 있다.
	+ switching context
	+ switching to user mode
	+ jumping to the proper location in the user program to restart that program
+ dipatcher가 실행 중이던 프로세스를 멈추고 다른 프로세스를 실행하는데 걸리는 시간을 Dispatcher latency라고 한다. 
## Criteria
다음과 같은 요소들을 기준으로, [[Scheduling Algorithm]]의 성능을 판단할 수 있다. 
+ CPU utilization: 단위 시간 동안 CPU가 작업을 수행한 시간의 비율
+ Throughput: 단위 시간 동안 실행을 완료한 프로세스의 수 
+ Turnaround time: 프로세스가 제출된 시간으로부터 실행이 완료될 때까지 시간$$Turnaround\; Time = time(new\rightarrow ready)+ waiting\; time+running \; time+time(doing\; I/O)$$
+ Waiting time: 프로세스가 ready queue에서 기다리는 시간
+ Response time: interactive system에서 요청이 제출되고, 첫 번째 반응이 올 때까지 걸리는 시간(time-sharing 환경)
## [[Scheduling Algorithm]]
![[Scheduling Algorithm]]