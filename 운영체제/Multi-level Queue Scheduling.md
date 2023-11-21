## Multi-level Queue Scheduling
---
Ready queue를 여러 개의 level로 쪼개서 구성해 각 queue에 다른 [[Scheduling Algorithm]]을 적용하는 한다. 각 queue는 일반적으로 우선순위를 정해 계층화되어 있다. 

각 queue에 대한 [[Scheduling Algorithm]]이 필요하고, 일반적으로 크게 두 가지 방법으로 구현할 수 있다.
+ Fixed prioirty scheduling: 각 queue의 우선순위가 고정되어, 높은 우선순위의 queue가 빈 후에 다음 우선순위의 queue 내부 프로세스가 실행될 수 있다. 이로 인해 [[Starvation]]문제의 가능성이 존재한다.
+ Time slice scheduling: 일정 시간 단위를 나눠 각 queue에 시간을 할당해 그 시간 동안에 각 queue 내부의 process가 CPU를 할당 받을 수 있도록 하는 방법. 
## Multi-level Feedback Queue Scheduling
---
위 방법에서 각 queue에 대해 [[Scheduling Algorithm]]를 적용하는 것이 아니라, *aging*을 통해 queue 내부의 process들이 다른 queue로 이동하는 방법.

이 방법에서 [[Process Scheduler]]는 다음과 같은 매개변수로 정의된다.
+ numbers of queues
+ [[Scheduling Algorithm]] for each queue
+ [[Scheduling Algorithm]] between the queues
+ method used to determine when to promote a process
+ method used to determine when to demote a process
+ method used to determine which queue a process will enter when that process needs service

### 예시
+ ready queues
	+ $Q_0$: [[RR(Round-Robin Scheduling)]] with time quantum 8ms
	+ $Q_1$: [[RR(Round-Robin Scheduling)]] with time quantum 16ms
	+ $Q_2$: [[FCFS(First-Come, First-Served Scheduling)]]
+ scheduling
	+ 새로운 프로세스는 $Q_0$로 진입. $Q_0$의 time quantum안에 작업이 완료되지 않으면, $Q_1$로 이동
	+ $Q_1$의 time quantum안에 작업이 완료되지 않으면 $Q_2$로 이동