각 프로세스에게 짧은 시간 단위의 CPU time(time quantum)을 할당해 그 시간 동안 돌아가면서 CPU를 할당하는 [[Scheduling Algorithm]]

이때 time quantum 값은 일반적으로 10-100ms이다. 할당 받은 time quantum이 종료되면 할당 받은 time quantum이 아직 만료되지 않은 다음 프로세스가 실행된다. time quantum이 존재하는 프로세스 간에는 [[FCFS(First-Come, First-Served Scheduling)]] 혹은 [[Priority Scheduling]]을 적용해 작동한다.

Ready queue에 $n$개의 프로세스가 있고, time quantum을 $q$라고 하면, 각 프로세스는 $(n-1)\times q$이상 기다리지 않는다.

성능은 time quantum에 의해 결정되며, $q$가 크면 [[FCFS(First-Come, First-Served Scheduling)]]와 동일한 성능을 가지고, $q$가 작으면 높은 동시성을 가진다. 

이 방법에서는 일반적으로 $q$가 작을 수록 [[Context Switch]]가 빈번하게 발생하게 발생하는 문제가 생긴다. [[Context Switch]]하는 시간은 overhead이기 때문에 이 수가 증가하게 되면 성능은 오히려 줄어들게 된다. 이로 인해 적절한 $q$를 결정하는 것이 중요하며, 현재 대부분의 OS는 10-100ms로 $q$를 결정한다. 

또한, ime qunantum은 turnaround time과 연관이 있는데, $q$가 커질수록 평균 tt는 작아지는 경향을 보인다. 