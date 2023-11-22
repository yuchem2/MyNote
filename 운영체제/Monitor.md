[[Process Synchronization]]을 한 편리하고, 효과적인 high-level abstraction로, [[객체 지향 프로그래밍 언어]]에서 Abstract data type 정의되어 있다.
```pseudo code
monitor monitor-name {
	// private data
	shared variable declartions 

	// public method
	procedure P1 (...) {...}
			...
	procedure Pn (...) {...}

	// initalization method
	Initialization code (...) {...}
}
```

Monitor instance는 여러 [[Process]]에서 공유된다. 한 순간에 오직 하나의 프로세스만이 monitor와 함께 활성화 될 수 있음을 보장한다. 
## Schematic View
### Basic Monitor
Monitor의 구성 요소는 다음과 같다.
+ Shared data: operation에 의해 접근될 수 있다. 
+ Opearations(method)
+ Initialization code
+ Entry queue: monitor 내 procedure만큼 존
### With Condition Variables
Basic Monitor는 [[Process Synchronization]] schems을 모델링하는 데 충분히 강력하지 않음. 이를 해결하기 위해 moniter에 `condition` variables을 추가
+ `condition x, y`: 프로그래머는 하나 이상의 `condition` type 변수를 정의할 수 있음
+ `x.wait()`: 작업을 호출한 프로세스는 일시 정지
+ `x.signal()`: `x.wait()`을 실행시켰던 하나의 프로세스를 다시 시작 시킴. 

`x.siganl()`이 프로세스 P에 의해 실행될 때 `condition x`에서 일시 정지되어 있는 프로세스 Q가 있는 상황이라고 하면, 프로세스 Q가 그 실행을 재개하는 것이 허용되는 경우 프로세스 P는 기다려야 한다. 그렇지 않으면, P와 Q는 동시에 활성화될 것이다. 
+ Signal and Wait: P는 Q가 monitor를 떠나거나 다른 `condition`에 의해 기다릴 때까지 기다린다. 
+ Signal and Continue: Q는 P가 monitor를 떠나거나 다른 `condition`에 의해 기다릴 때까지 기다린다. 
#### Solution to [[Dining-Philosophers Problem]]
```c++
monitor DP {
	enum { THINKING, HUNGRY, EATING } state[5];
	condition self[5]; // philosopher

	void pickup(int i) {
		state[i] = HUNGRY;
		test(i);
		// 자신이 eating하지 못했으면 일시 정지
		if(state[i] != EATING) 
			self[i].wait(); 
	}

	void putdown(int i) {
		state[i] = THINKING;
		// 두개의 젓가락 놓기
		test((i + 4) % 5); // 왼쪽 philosopher가 eating할 수 있도록 
		test((i + 1) % 5); // 오른쪽 philosopher가 eating할 수 있도록
	}

	void test(int i) {
		// 자신의 왼쪽, 오른쪽 philosopher가 eating이 아니고, 자신이 hungry일 때
		if((state[(i + 4) % 5] != EATING) && state[i] == HUNGRY 
			&& STATE[(i + 1) % 5] != EATING) {
			state[i] = EATING; // 자신의 상태를 EATING으로 바꿈
			self[i].signal(); // 일시정지되어 있는 경우 활성화
		}
	}

	initialization_code() {
		for(int i=0; i<5; i++)
			state[i] = THIKING;
	}
}

// each philosoher i
do {

	// thinking
	DP.pickup(i);
	// eat
	DP.putdown(i);
	// thinking
	
} while(true)
```
## 장단점
### 장점
+ 사용이 쉬움
+ [[Deadlock]] 등 error 발생 가능성이 낮음
### 단점
+ 지원하는 언어에서만 사용 가능
+ [[컴파일러(Compiler)]]가 [[운영체제(Operating System)]]를 이해하고 있어야 함
## Implementation
### Definition of Monitor
```c
Monitor M {
	condition x;
	int a, b, c,  d;

	void F1() { body of F1; }
	void F2() { body of F2; }
	void F3() { body of F3; }

	intialization() { init; }
}
```
### Using [[Semaphore]]
```C++
class M {
	sem_t mutex, next, x_sem;
	int next_count, x_count; // next_count, x_count: condition varables
	int a, b, c, d;

	void x_wait() { ; }
	void x_signal() { ; }

	void F1() {
		//...
		// body of F1
		;
		// ...
	}
	
	void F2() {
		//...
		// body of F2
		;
		// ...
	}
	
	void F3() {
		//...
		// body of F3
		;
		// ...
	}

	MonitorM() { init; }
}
```
#### Variables
```c
sem_t mutex; // initialized to 1.
sem_t next;  // initialized to 0. 
int next_count = 0; // x_signal()에 의해 block되어있는 프로세스 수
sem_t x_sem; // initialized to 0.
int x_count = 0; // x_wait()에 의해 block되어있는 프로세스 수
```
#### Each External procedure F
```c
void F1() {
	// ...
	wait(mutex);
	
	// body of F
	
	if (next_count > 0) // wait하고 있는 process가 있는 경우 재개시킴
		signal(next);
	else 
		signal(mutex);
	// ...
}

```
#### Implementation of x.wait()
```c
void x_wait() {
	x_count++;
	if (next_count > 0) // x.signal()에 의해 block되어 있는 프로세스가 있는 경우
		signal(next);
	else
		signal(mutex);
	wait(x_sem);
	x_count--;
}
```
#### Implementation of x.signal()
```c
void x_signal() {
	if (x_count > 0) { // 프로세스가 condition x에 의해 block된 경우만 실행
		next_count++;  // x.signal()에 의해 block되어 있는 프로세스 수 
		signal(x_sem); // x_sem에 의해 block되어 있는 프로세스를 깨움
		wait(next);   // 깨워진 프로세스가 monitor function을 종료할 때까지 자신 block
		next_count--;
	}
}
```