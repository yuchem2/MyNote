동시에 수행되는 많은 프로세스들에 의해 데이터 집합이 공유된다. 
+ Readers: 데이터 집합을 읽기만 하는 형태. 어떤 변경도 수행 x
+ Writers: 읽고 쓸 수 있다. 

이러한 상황에서 다수의 reader가 동시에 읽는 것이 허용되고, 오직 하나의 writer만이 데이터에 접근 가능할 때 데이터의 일관성이 유지되는 것을 해결하는 문제

다음과 같은 공유 데이터를 통해 문제를 해결할 수 있다.
+ [[Semaphore]] `mutex`: initalized to the value 1
+ [[Semaphore]] `wrt`: initailized to the value 1
+ Integer `readcount`: intialized to the value 0
#### Structure of Writer Process
```c
do {
	wait(wrt); // enter a cs
	
	// writing 

	signal(wrt); // exit section
	
} while(true);
```
#### Structure of Reader Process
```c
do {
	wait(mutex);
	readcount++;
	if(readcount==1) wait(wrt); // when it is the first reader
	signal(mutex); // 동시에 readcount를 변경하는 것을 막기 위함

	// reading

	wait(mutex); // 동시에 readcount를 변경하는 것을 막기 위함
	readcount--;
	if(readcount==0) signal(wrt); // when it is the last reading
	signal(mutex);
	
} while(true);
```
#### [[Starvation]]
이 문제에서 [[Starvation]] 문제가 생긴다. 여기서 writer는 [[Starvation]] 상태를 가질 수 있다. 계속해서 새로운 reader가 reading 상태에 진입하면 writer는 공유 데이터를 영원히 못 읽을 수 있다.

writer가 준비된 경우 새로운 reader의 접근을 막도록 문제를 변형한 경우에는 reader가 [[Starvation]]이 될 수 있다. writer가 접근을 기다리게 되면 더 이상 새로운 reader가 reading 상태에 진입하지 못하고 기다리는 상태가 된다. 