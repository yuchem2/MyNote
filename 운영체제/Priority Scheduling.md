각 프로세스에 우선순위 번호를 주어 우선순위에 따라 CPU를 할당하는 [[Scheduling Algorithm]]. 일반적으로 번호가 작을 수록 높은 우선순위라고 판단한다.

[[Process Scheduling#Non-preemptive]], [[Process Scheduling#Preemptive]] 방식으로 모두 구현할 수 있다

[[SJF(Shortest-Job-First Scheduling)]]도 일종의 [[Priority Scheduling]]이라고 할 수 있다

*Starvation* 문제가 생길 수 있다. 높은 우선순위 프로세스가 계속해서 들어오면, 상대적으로 낮은 우선순위의 프로세스는 계속 실행되지 못하고 ready queue에서 기다리는 경우가 발생한다.

이 문제는 *Aging*을 통해 해결할 수 있다. 이 방법은 ready queue에서 프로세스가  기다리는 시간이 증가하면 그 프로세스의 우선순위를 높임으로서 낮은 우선순위의 프로세스가 계속 기다리는 상태를 방지한다. 