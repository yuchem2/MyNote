
CPU를 여러 [[Process]] 중 어떤 process에 할당할지 결정하는 작업

각 목적에 따라 어떻게 실행시킬지 결정한다
+ [[Multi-programming]]의 목적은 CPU utilization을 최대화하는 것임으로, 항상 process들을 실행시키고자 한다
+ [[Multitasking]]의 목적은 실제로 동시에 실행하는 것처럼 보이기 위함으로, CPU를 process간 계속 switch한다

[[큐(Queue)]]를 기반으로 작동하며 다음과 같은 큐들을 가지고 있다.
+ Job queue: 시스템 내 모든 프로세스 집합
+ Ready queue: ready state인 process들의 집합
+ Device queue: wait state인 process들 중 I/O를 기다리고 있는 process들의 집합
	+ 각 Device들은 고유한 자신의 queue를 가지고 있음


