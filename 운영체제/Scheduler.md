
다양한 [[큐(Queue)]]에 있는 [[Process]]들 집합에서 실행시킬 process를 선택하는 역할을 수행하는 [[운영체제(Operating System)]]의 구성요소

Batch or [[Multi-programming]] 시스템에서 즉시 실행될 수 있는 것보다 많은 수의 process들이 메모리에 올라와 대기되고, 실행을 위해 디스크에 보관된다. 이러한 프로세스들을 관리하는 역할을 수행한다

종류
+ [[Long-term Scheduler(Job Scheduler)]]
+ [[Sort-term Scheduler(CPU Scheduler)]]

유닉스와 Window XP와 같은 [[Time-sharing]] 시스템에서는 [[Long-term Scheduler(Job Scheduler)]]는 사용되지 않고, 모든 새로운 [[Process]]는 단순히  [[Sort-term Scheduler(CPU Scheduler)]]에 의해 메모리에 올려진다

그러므로 안정성이 물리적인 제약이나 사용자의 적응 정도에 의존하게 된다. 또한, [[Mid-term Scheduler]]가 사용된다