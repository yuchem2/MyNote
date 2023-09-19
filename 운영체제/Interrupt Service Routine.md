
각 컴퓨터마다 특정한 [[인터럽트(Interrupt)]] 처리에 관한 mechanism을 가지만, 몇 가지 기능은 동일하게 수행한다. 

+ Generic routine
+ Interrupt-specific handler

일반적인 interrrup service routine은 다음과 같이 진행된다.
1. [[인터럽트(Interrupt)]] 정보를 점검하기 위해 generic routine을 호출한다.
2. 그 후 [[인터럽트(Interrupt)]]에 따라 맞는 interrupt-specific handler가 호출된다

이러한 처리는 가능한 빨리 처리가 되어야 한다. (정해진 일정 수의 [[인터럽트(Interrupt)]]만 가능)

