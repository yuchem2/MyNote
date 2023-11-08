가장 간단한 형태의 [[Scheduling Algorithm]]로, 단순히 ready queue에 진입한 순서대로, [[Process]]에게 CPU를 할당한다.

이로 인해 알고리즘을 통해 성능이 조절되지 않고, 단순히 각 프로세스가 어떤 순서로 ready queue에 진입했는지에 따라 [[Process Scheduling#Criteria]]가 결정된다.

하지만 각 프로세스의 시점에서 봤을 때 비교적 빠르게 결과를 얻을 수 있다는 장점을 가지고 있다. 

[[Process Scheduling#Non-preemptive]]로 구분할 수 있고, 이에 따라 time-sharing system에서는 사용이 불가능하다.

*Convoy effect*가 발생할 수 있다. 이는 많은 process가 줄지어서 기다리고 있는 상태로, 만약 하나의 CPU bound process가 실행되고 있다면, I/O bound process는 실행조차 하지 못하고 기다리는 상태로 있게 된다. 이로 인해 총 CPU utilization이 떨어지는 결과를 이끌어 낸다. 