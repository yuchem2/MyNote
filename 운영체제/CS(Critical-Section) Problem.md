n개의 [[Process]] $\{P_0, P_1, ..., P_{n-1}\}$로 이루어진 시스템에서 각 프로세스는 critical section으로 불리는 코드 부분을 가지고 있다. 이 구역은 공유 변수를 변경하거나, 테이블을 수정하거나, 파일을 쓰는 등의 작업을 말한다. 이 경우에 두 개 이상의 프로세스가 critical section을 동시에 수행되지 않게 해야 하는 문제이다.

## Solution
---
프로세스들이 협력할 수 있는 [[Protocol]]을 만든다. 
+ 각 프로세스는 CS에 들어갈 때 승인을 요청해야 한다.
+ 이 요청을 구현하는 부분은 entry section로, CS의 앞 부분에 위치한다. 다른 프로세스가 CS을 수행하고 있는지 확인하는 부분이다. 
+ CS의 뒷 부분은 exit section으로, CS의 수행이 완료되었음을 다른 프로세스들에게 알린다.
+ 이 외 코드 영역은 reminder section이라고 한다.
```c
// The genral structure of a a typical processs Pi
Do {
	/*
		Entry Section
	*/

	/*
		Critical Section
	*/

	/*
		Exit Section
	*/

	/*
		Remainder Section
	*/
} while (TRUE);
```
## Example program for CS
---
```c
#define MAX_LIST 10

int int_array[MAX_LIST]
struct timespace tv;
time_t init_time;

int main(int argc, char** argv) {
	pthread_t a_thread=0, b_thread=0;
	int ret, a_interval, b_interval;

	init_time = time(NULL);
	tv.tv_sec = 0;
	tv.tv_nsec = 10 * 1000 * 1000;
	srand(time(NULL));

	printf("####### Thread Test: START......\n");
	a_interval = 1;
	ret = pthread_create(&a_thead, NULL, (void*)sorting_thread, &a_interval);
	b_interval = 2;
	ret = pthread_create(&b_thead, NULL, (void*)sorting_thread, &b_interval);

	pthread_join(a_thread, NULL);
	pthread_join(b_thread, NULL);
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

void random_function() {
	int i;
	for (i=0; i<MAX_LIST; i++)
		int_array[i] = rand()%10;
}

void sorting_function() {
	int i, j, tmp;

	// total execution time = 450 millieconds
	for(i=MAX_LIST-1; i>0; i--) {
		for(j=0; j<i; j++) {
			nanosleep(&tv, NULL); // 10 milliseconds sleep
			if(int_array[j] > int_array[j+1]) {
				tmp = int_array[j+1];
				int_array[j+1] = int_array[j];
				int_array[j] = tmp;
			}
		}
	}
}

void print_array(int no) {
	int i, flag = 0;

	printf("[%d]-->", no);
	for(i=0; i<MAX_LIST-1; i++) {
		printf("%5d", int_array[i+1]);
		if(int_array[i] > int_array[i+1]) 
			flag = 1;
	}
	printf("%5d", int_array[i]);
	flag ? printf(" [x] ") : printf(" [] ");
}
```

