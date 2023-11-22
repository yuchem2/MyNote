HW 지원 명령어들은 응용 프로그래머가 사용하기에는 복잡하다. 이 문제를 해결하기 위해 사용되는 정수 변수이다. Semaphore는 busy waiting을 필요로 하지 않는 [[Process Synchronization]] 툴. 

S로 표기하며 표준 연산 `wait()`과 `signal()`이 사용되며 이 연산은 ATOMIC하다. 
## Operations
코드는 실제 구현 내용이 아니라 동작 개념도 이다. 한 프로세스가 semaphore의 값을 수정할 때 다른 프로세스는 semaphore값을 수정하지 못한다.
### Wait(S)
```c
wait (S) {
	while (S <= 0) ; // busy-waiting
	S--;
}
```
### Signal(S)
```c
signal(S) {
	S++;
}
```
### Usage
#### Counting semaphore
특정한 구간에 대해 정의해 사용하는 방법 e.g. 0..10
##### Resoure Allocation
주어진 유한한 개수의 자원에 접근을 제어하는 데 사용된다. 
1. Semaphore를 사용 가능한 자원의 수로 초기화. 
2. 자원을 사용하기 위해 프로세스는 wait() 호출
3. 자원을 내어놓기 위해 프로세스는 signal() 호출
4. Semaphore가 0일 때 모든 자원이 사용되고 있음을 의미
##### Syncronization 해결
다양한 동기화 문제를 해결 가능하다. 

e.g. 동시에 수행되는 두 개의 프로세스 P1, P2가 존재할 때 각각 S1, S2 statement를 가질 때 S2가 S1이 수행되고 나서 수행되고자 할 때 sempahore를 이용해 해결 가능
+ Initialization: `sempahore synch = 0;`
+ P1 structure: `S1; singal(sync);`
+ P2 structure: `wait (synch); S2;`

나머지 함수 부분은 [[CS(Critical-Section) Problem#Example program for CS]]과 동일
```c
#define MAX_LIST 10

int int_array[MAX_LIST]
struct timespace tv;
static sem_t a_sem, b_sem;

int main(int argc, char** argv) {
	pthread_t r_th, s_th, p_th;
	int ret, i, r_ID, s_ID, p_ID;
	
	tv.tv_sec = 0;
	tv.tv_nsec = 10 * 1000 * 1000;
	srand(time(NULL));
	ret = sem_init(&a_sem, 0, 0);
	ret = sem_init(&b_sem, 0, 0);

	printf("####### Thread Test: START......\n");
	p_ID = 3;
	s_ID = 2;
	r_ID = 1;
	
	ret = pthread_create(&(p_th), NULL, (void*)printing_thread, &(p_ID));
	ret = pthread_create(&(s_th), NULL, (void*)printing_thread, &(s_ID));
	ret = pthread_create(&(r_th), NULL, (void*)printing_thread, &(r_ID));

	pthread_join(p_th, NULL);
	pthread_join(s_th, NULL);
	pthread_join(r_th, NULL);
	
	sem_destroy(&a_sem);
	sem_destroy(&b_sem);
	printf("###### Thread Test: END......\n");
}

void sorting_thread(void* p_data) {
	int i=0, max=10;
	int interval = *((int*)p_data);
	struct timeval start, end;

	printf("###### Thread #%d started......\n", id);
	for(i=0; i<max; i++) {
		sleep(interval);

		gettimeofday(&start, NULL);
		random_function();
		sorting_function();
		print_array(interval);
		gettimeofday(&end, NULL);

		printf("%2d:%6d -> %2d:%6d\n", 
			start.tv_sec-init_time, start_tv_usec, end.tv_sec-init_time, end.tv_usec);
	}
}

void random_thread(void* p_data) {
	int i, max = 10;
	int id = *((int*)p_data);
	
	printf("###### Thread #%d started......\n", id);
	sleep(1);
	for (i=0; i<max; i++) {
		random_function();
		sem_post(&a_sem);
	}		
}

void sorting_thread(void* p_data) {
	int i, max = 10;
	int id = *((int*)p_data);
	
	printf("###### Thread #%d started......\n", id);
	sleep(1);
	for (i=0; i<max; i++) {
		sem_wait(&a_sem);
		sorting_function();
		sem_post(&b_sem);
	}	
}

void print_array(void* p_data) {
	int i, max = 10;
	int id = *((int*)p_data);
	
	printf("###### Thread #%d started......\n", id);
	sleep(1);
	for (i=0; i<max; i++) {
		sem_wait(&b_sem);
		print_array(id, i);
	}	
}
```
#### Binary semaphore
0과 1의 값만 가지게 사용하는 방법. *mutex locks*라고 불린다.
##### CS 문제 해결
n개의 프로세스가 semaphore `mutex`를 공유하고, 1로 초기화해 사용한다. $P_i$의 기본 구조는 다음과 같다.
```c
do {
	wait(mutex);
	/*
		Critical Section
	*/
	signal(mutex);
	/*
		Remainder Section
	*/
} while(TRUE);
```

##### C 언어 예시
나머지 함수 부분은 [[CS(Critical-Section) Problem#Example program for CS]]과 동일
```c
#define MAX_LIST 10

int int_array[MAX_LIST]
static sem_t my_sem; // semaphore
struct timespace tv;
time_t init_time;

int main(int argc, char** argv) {
	pthread_t a_thread=0, b_thread=0;
	int ret, a_interval, b_interval;

	init_time = time(NULL);
	tv.tv_sec = 0;
	tv.tv_nsec = 10 * 1000 * 1000;
	srand(time(NULL));

	ret = sem_init(&my_sem, 0, 1); // 1로 초기화
	printf("####### Thread Test: START......\n");
	a_interval = 1;
	ret = pthread_create(&a_thead, NULL, (void*)sorting_thread, &a_interval);
	b_interval = 2;
	ret = pthread_create(&b_thead, NULL, (void*)sorting_thread, &b_interval);

	pthread_join(a_thread, NULL);
	pthread_join(b_thread, NULL);
	printf("###### Thread Test: END......\n");

	sem_destroy(&my_sem); // semaphore 해제
}

void sorting_thread(void* p_data) {
	int i=0, max=10;
	int interval = *((int*)p_data);
	struct timeval start, end;

	printf("###### Thread #%d started......\n", id);
	for(i=0; i<max; i++) {
		sleep(interval);

		sem_wait(&my_sem); // Entry Section, wait()

		// Critical Section
		gettimeofday(&start, NULL);
		random_function();
		sorting_function();
		print_array(interval);
		gettimeofday(&end, NULL);

		sem_post(&my_sem); // Exit Section, signal()
		
		printf("%2d:%6d -> %2d:%6d\n", 
			start.tv_sec-init_time, start_tv_usec, end.tv_sec-init_time, end.tv_usec);
	}
}
```

위 예시에서 Mutual exclusion과 progress는 만족하지만 기본적으로 bounded waiting은 만족하지 않는다. 일반적으로 `wait()` 함수의 구현에 의존적으로 구성된다. e.g. Linux의 `sem_wait()`은 이를 보장하지 않음.
### Implementation 
#### with busy waiting
동시에 프로세스들이 `wait()`과 `signal()`을 수행하지 않도록 보장되어야 한다. 그러므로 구현은 `wait`과 `signal`가 CS에 위치하는 CS문제로 볼 수 있다.

이전 코드들은 모두 CS에 있어서 *busy waiting*를 수행하고 있다. *busy waiting*은 *spinlock*이라고도 한다. 한 프로세스가 CS에 있는 동안 다른 프로세스들은 계속해서 `wait`을 반복하게 된다. 
 + 단점: `wait`을 수행하는 동안 CPU cycle이 낭비
 + 때때로 유용: `wait`, `signal`을 위한 context switch가 필요 없음. 구현할 코드가 짧음. CS가 드물면 *busy waiting*이 거의 발생하지 않음

그러나 응용프로그램들이 CS에서 많은 시간을 소비할 수 있기 때문에 좋은 해결 방안이 아님
#### without busy waiting
두 개의 연산을 사용해 *busy waiting*을 해결할 수 있다.
+ `block()`: 연산을 호출한 프로세스를 적당한 waiting queue.
+ `wakeup()`: waiting queue에 있는 여러 프로세스 중 하나를 제거해 ready queue로 옮김
+ semaphore 값이 `wait()`을 수행하는 데 양수가 아닐 때 스스로를 block
+ `signal()` 연산은 대기 중인 프로세스를 wake up
위 방법을 통해 semaphore를 구현하기 위해 다음 레코드 형식으로 정의한다. 
```c
typedef struct {
	int value; // semaphore
	struct process *list; // pointer to the PCB list of waiting processes
}
```
각 semaphore에는 연관된 waiting queue가 존재한다. 이때 waiting queue는 다음과 같이 구현한다. 
+ 연결 리스트로 구현
+ 대기 중인 프로세스들은 PCB를 포함
+ 음수 값은 대기하고 있는 프로세스의 수를 의미
+ 양수 값은 이용 가능한 자원 개수를 의미
+ 리스트에 다양한 queuing strategy를 적용 가능
+ 유한 대기 조건을 만족시키기 위하여 큐는 FIFO 큐로 구현할 수 있음. (PCB 리스트의 처음과 끝을 가리키는 두 개의 포인터 변수를 사용)
##### wait()
```pseudo code
wait(semaphore *S) {
	S->value--;
	if(S->value < 0) {
		add this process to S->list;
		block();
	}
}
```
##### signal()
```pseudo code
signal(semaphore *S) {
	S->value++;
	if(S->value <= 0) {
		remove a process P from S->list;
		wakeup();
	}
}
```
## 해결할 수 있는 문제
### [[Producer-Consumer Problem]] with Bounded-Buffer
다음과 같은 상태의 [[Producer-Consumer Problem]]가 있다. 
+ 두 개 이상의 생산자, 두 개 이상의 소비자
+ 버퍼는 최대 N개의 아이템을 포함

이 문제는 [[Semaphore]]를 통해 해결할 수 있다. 각 [[Semaphore]]를 다음과 같이 초기화한다.
+ [[Semaphore]]`mutex`: initalized to the value 1
+ [[Semaphore]] `full`: initailized to the value 0
+ [[Semaphore]] `empty`: intialized to the value N
#### Structure of Producer Process
```c
do {
	// produce an item
	wait(empty); // check buffer is empty
	wait(mutex); // enter cs

	// add the item to the buffer
	signal(mutex); // exit cs
	signal(full); // one item is produced
	
} while(true);
```
#### Structure of Consumer Process
```c
do {
	wait(full); // check buffer is empty or not 
	wait(mutex); // enter cs

	// cs

	signal(mutex); // exit cs
	signal(empty); // one item is produced

} while(true);
```
### [[Readers and Writers Problem]]
![[Readers and Writers Problem]]
### [[Dining-Philosophers Problem]]
![[Dining-Philosophers Problem]]
## 발생할 수 있는 문제
사용할 때 잘못 사용한다면 문제가 생길 수 있다. 에러를 처리하기 위해 [[Monitor]]가 사용될 수 있다.

e.g.`mutex`는 1로 초기화하는 semaphore인데, 이를 다음과 같이 사용하면 오류가 생길 수 있다.
+ `signal(mutex) ... wait(mutex)`: mutex exclusion 위반
+ `wait(mutex) ... wait(mutex)`: [[Deadlock]] 발생
+ `wait(mutex)` or `signal(mutex)` 생략: mutex exclusion 위반 or [[Deadlock]] 발생