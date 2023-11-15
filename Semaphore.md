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

위 예시에서 Mutual exclusion과 progress는 만족하지만 기본적으로 bounded waiting은 만족하지 않는다. 일반적으로 wait() 함수의 구현에 의존적으로 구성된다. e.g. Linux의 sem_wait()은 이를 보장하지 않음.