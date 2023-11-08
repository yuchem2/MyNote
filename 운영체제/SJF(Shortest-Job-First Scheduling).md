가장 이상적으로 평균 waiting time을 최소화는 [[Scheduling Algorithm]]으로, ready queud에 있는 프로세스 중 다음 CPU burst의 길이가 가장 짧은 프로세스에게 CPU를 할당한다.

[[Process Scheduling#Non-preemptive]], [[Process Scheduling#Preemptive]] 방식으로 모두 구현할 수 있다
## Non-preemptive SJF
---
한번 CPU를 할당 받으면 CPU burst를 수행할 때까지 내려오지 않고, 계속되어 실행.

## Preemptive SJF
---
ready queue에 새로운 process가 들어올 때마다 [[Process Scheduler]]가 작동해 현재 running하고 있는 process를 ready queue에 이동시킨 다음 알고리즘을 적용해 다시 가장 짧은 다음 CPU burst를 가진 프로세스에게 CPU할당

이러한 방식으로 인해 CPU burst가 쪼개져서 실행될 수 있으며 그로 인해 SRTF(Shortest-Remaining-Time-First)라고도 말한다.

이 방식이 평균 waiting time을 가장 최소화하는 optimal한 [[Scheduling Algorithm]]이다.

## Determining Length of Next CUP Burst
---
이 알고리즘에서는 프로세스를 선택하기 위해 각 프로세스의 다음 CPU Burst를 알아야 한다. 하지만 이 값은 실제로 실행되기 전까지는 측정할 수 없어 특정 값으로 예측해 사용한다. 

이때 값을 예측할 때 이전 CPU Burst를 이용해 expoential averaging 기법을 통해 구한다. $t_n$을 $n$번째 실제 CPU burst, $\tau_{n+1}$를 예측하는 다음 CPU Burst라고 정의하고, $0\leq\alpha\leq1$인 실수 $\alpha$를 정의한다. $$\tau_{n+1} = \alpha \times t_n + (1-\alpha)\times \tau_n$$
여기서 구한 $\tau_{n+1}$를 위 알고리즘을 수행할 때 적용해 프로세스를 선택한다.