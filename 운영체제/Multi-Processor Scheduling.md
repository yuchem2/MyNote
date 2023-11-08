CPU간의 프로세스 할당을 정의하는 [[Scheduling Algorithm]]으로, [[Multi-processor system]]의 종류에 따라 다르게 적용할 수 있다.
+ AMP: 하나의 CPU에 의해 다른 CPU들이 프로세스를 할당 받는다. 오직 하나의 CPU만 system data structure에 접근할 수 있어 data sharing에 대한 문제를 쉽게 해결 가능하다.  
+ SMP: 각 프로세스는 각각 고유한 [[Scheduling Algorithm]]을 가지고 수행된다. 여기서 모든 CPU가 동일한 부하를 갖도록 프로세스들이 할당된다. 
	+ Push migration: 자신의 queue에 많은 프로세스가 있는 경우 다른 CPU로 프로세스를 보냄
	+ Pull migration: 자신의 queue에 프로세스가 없거나 적은 경우 다른 CPU로 부터 프로세스를 가져옴