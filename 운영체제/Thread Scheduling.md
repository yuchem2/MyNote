[[Mulit-threaded process]]를 지원하는 [[운영체제(Operating System)]]에서는 OS에 의해 각 thread들이 scheduling된다. 크게 다음과 같이 구분할 수 있다.
+ Local Scheduling: threads library가 어떤 thread에게 가용 LWP(kernel thread)를 할당할 지 결정
+ Global Scheduling: kernel이 다음에 수행될 kernel thread를 결정한다.