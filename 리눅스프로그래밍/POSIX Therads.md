Linux에서 [[C]] 언어를 통해 multi-thread 환경을 구성하기 위해서는 `pthread.h`과 `_REENTRANT` 매크로를 정의해야 한다. 또한 thread library를 link하기 위해 `-lpthread` 옵션을 제공해야 한다. 
#### Basics
##### Create Thread
```c
#include <pthread.h>

int pthread_create(pthread_t *thread, pthread_attr_t *attr, void *(*start_routine)(void *), void *arg);
void pthread_exit(void *retval);
int pthread_join(pthread_t th, void **thread_return);
```
+ `pthread_create()`: `start_routine`에 해당 thread가 수행할 함수를 넘겨주고, 함수의 파라미터는 `arg`를 통해 넘겨주며 thread를 생성한다.
+ `pthread_exit()`: thread가 종료될 때 자동으로 호출하는 함수
+ `pthread_join()`: 프로세스가 thread가 종료될 때까지 기다림
##### Cancel Thread
```c
#include <pthread.h>

int pthread_cancel(pthread_t thread);
int pthread_setcancelstate(int state, int *oldsate);
int pthread_setcanceltype(int type, int *oldtype);
```
+ `pthread_cancel()`: 주어진 thread를 취소
+ `pthread_setcancelstate()`: 첫 번째 인자는 `PTHREAD_CANCEL_ENABLE` 또는 `PTHREAD_CANCEL_DISABLE`이 가능하며, 각각 thread취소 명령을 승인하거나 무시하는 옵션이다. 
+ `pthread_setcanceltype()`: 첫 번째 인자는 `PTHREAD_CANCEL_ASYNCHRONOUS` 또는 `PTHREAD_CANCEL_DEFFERRED`로 각각 다음이 허용된다.
	+ `PTHREAD_CANCEL_ASYNCHRONOUS`: 취소 요청이 들어오면 즉시 취소
	+ `PTHREAD_CANCEL_DEFFERRED`: 취소 요청이 들어올 때 `pthread_join()`, `pthread_cond_wait()`, `pthread_cond_timedwait()`, `pthread_testcancel()`, `sem_wait()`, `sigwait()`을 수행하고 있으면 수행이 완료될 때까지 기다렸다가 취소
##### Example
###### thread1
```c
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
#include <string.h>
#include <pthread.h>

char message[] = "Hello World";

void *thread_function(void *arg) {
    printf("thread_function is running. Argument was %s\n", (char *)arg);
    sleep(3);
    strcpy(message, "Bye!");
    pthread_exit("Thank you for the CPU time");
}

int main() {
    int res;
    pthread_t a_thread;
    void *thread_result;
    res = pthread_create(&a_thread, NULL, thread_function, (void *)message);
    if (res != 0) {
        perror("Thread creation failed");
        exit(EXIT_FAILURE);
    }
    printf("Waiting for thread to finish...\n");
    res = pthread_join(a_thread, &thread_result);
    if (res != 0) {
        perror("Thread join failed");
        exit(EXIT_FAILURE);
    }
    printf("Thread joined, it returned %s\n", (char *)thread_result);
    printf("Message is now %s\n", message);
    exit(EXIT_SUCCESS);
}
```
###### Simultaneous Execution
```c
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
#include <pthread.h>

int run_now = 1;
char message[] = "Hello World";

void *thread_function(void *arg) {
    int print_count2 = 0;

    while(print_count2++ < 20) {
        if (run_now == 2) {
            printf("2");
            run_now = 1;
        }
        else {
            sleep(1);
        }
    }

    sleep(3);
}

int main() {
    int res;
    pthread_t a_thread;
    void *thread_result;
    int print_count1 = 0;

    res = pthread_create(&a_thread, NULL, thread_function, (void *)message);
    if (res != 0) {
        perror("Thread creation failed");
        exit(EXIT_FAILURE);
    }

    while(print_count1++ < 20) {
        if (run_now == 1) {
            printf("1");
            run_now = 2;
        }
        else {
            sleep(1);
        }
    }

    printf("\nWaiting for thread to finish...\n");
    res = pthread_join(a_thread, &thread_result);
    if (res != 0) {
        perror("Thread join failed");
        exit(EXIT_FAILURE);
    }
    printf("Thread joined\n");
    exit(EXIT_SUCCESS);
}
```
###### Canceling
```c
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
#include <pthread.h>

void *thread_function(void *arg) {
    int i, res, j;
    res = pthread_setcancelstate(PTHREAD_CANCEL_ENABLE, NULL);
    if (res != 0) {
        perror("Thread pthread_setcancelstate failed");
        exit(EXIT_FAILURE);
    }
    res = pthread_setcanceltype(PTHREAD_CANCEL_DEFERRED, NULL);
    if (res != 0) {
        perror("Thread pthread_setcanceltype failed");
        exit(EXIT_FAILURE);
    }
    printf("thread_function is running\n");
    for(i = 0; i < 10; i++) {
        printf("Thread is still running (%d)...\n", i);
        sleep(1);
    }
    pthread_exit(0);
}

int main() {
    int res;
    pthread_t a_thread;
    void *thread_result;

    res = pthread_create(&a_thread, NULL, thread_function, NULL);
    if (res != 0) {
        perror("Thread creation failed");
        exit(EXIT_FAILURE);
    }
    sleep(3);
    printf("Canceling thread...\n");
    res = pthread_cancel(a_thread);
    if (res != 0) {
        perror("Thread cancelation failed");
        exit(EXIT_FAILURE);
    }
    printf("Waiting for thread to finish...\n");
    res = pthread_join(a_thread, &thread_result);
    if (res != 0) {
        perror("Thread join failed");
        exit(EXIT_FAILURE);
    }
    exit(EXIT_SUCCESS);
}
```
###### 5 threads
```c
#include <stdlib.h>
#include <stdio.h>
#include <string.h>
#include <errno.h>
#include <unistd.h>
#include <math.h>
#include <fcntl.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <sys/time.h>
#include <unistd.h>
#include <signal.h>
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
#include <string.h>
#include <pthread.h>

/*
	thread 0
	thread 1
	thread 2
	thread 3
	thread 4
	thread 0
	thread 1
	thread 2
	thread 3
	thread 4
	thread 0
	thread 1
	thread 2
	thread 3
	thread 4
*/

#define MAX_COUNT	3
#define MAX_THREAD	5

int run_now = 0;

void *thread_function(void *arg) {
	int i = 0;
	while (i < MAX_COUNT) {
		if (run_now == (int *)arg)	{
			printf("thread %d\n", (int *)arg);
			run_now = (run_now+1)%MAX_THREAD;
			i++;
		}
	}

}

int main(int argc, char **argv) {
    int res, i;
    pthread_t a_thread[MAX_THREAD];
	int arg[MAX_THREAD];

	if (argc != 1) {
		printf("USAGE: ] ./ret \n");
		exit(0);
	}

	struct timeval tv1, tv2;
	gettimeofday( &tv1, NULL);
	printf("\ntime = %10d.%03d\n\n", tv1.tv_sec, tv1.tv_usec/1000);

	for (i=0; i<MAX_THREAD; i++) {
		arg[i] = i;
		res = pthread_create(&(a_thread[i]), NULL, thread_function, (void*)arg[i]);
		if (res != 0) {
			perror("Thread creation failed");
			exit(EXIT_FAILURE);
		}
		usleep(100);
	}

	for (i = 0; i<MAX_THREAD; i++) {
		res = pthread_join(a_thread[i], &thread_result);
		if (res != 0) {
			perror("pthread_join failed");
		}
	}

	gettimeofday( &tv2, NULL);
	printf("\n\ntime = %10d.%03d\n\n", tv2.tv_sec, tv2.tv_usec/1000);

	if( tv2.tv_usec >= tv1.tv_usec)
		printf("Execution Time: %10d.%03d\n\n", 
		tv2.tv_sec - tv1.tv_sec, (tv2.tv_usec - tv1.tv_usec)/1000 );
	else
		printf("Execution Time: %10d.%03d\n\n", 
		tv2.tv_sec - tv1.tv_sec - 1, (1000000 + tv2.tv_usec - tv1.tv_usec)/1000 );

	printf("Execution finished successfully.... \n\n");
}
```
###### Matrix Multiplication
```c
#include <stdlib.h>
#include <stdio.h>
#include <string.h>
#include <errno.h>
#include <unistd.h>
#include <math.h>
#include <fcntl.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <sys/time.h>
#include <unistd.h>
#include <signal.h>
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
#include <string.h>
#include <pthread.h>

#define MAX_VALUE	3
#define MAX_ROW		100
#define MAX_COLUMN	100
#define MAX_THREAD (MAX_ROW	* MAX_COLUMN)

int g_Ma[MAX_ROW][MAX_COLUMN], g_Mb[MAX_ROW][MAX_COLUMN], g_Mab[MAX_ROW][MAX_COLUMN];
int g_row=0, g_column=0;


void print_matrix(int M[MAX_ROW][MAX_COLUMN], int max_row, int max_column) {
	int row, column;
	
	printf("\n");	
	for(row=0; row<max_row; row++) {
		for(column=0; column<max_column; column++)
			printf("%4d", M[row][column]);
		printf("\n");
	}

}

void make_matrix(int M[MAX_ROW][MAX_COLUMN], int max_row, int max_column) {
	int row, column;

	for(row=0; row<max_row; row++)
		for(column=0; column<max_column; column++) {
			//M[row][column] = 1; 
			M[row][column] = rand()%MAX_VALUE;
		}
}

int single_multiflication(int row, int column) {
	int value = 0;

	for(int i=0; i<g_column; i++) {
		value += g_Ma[row][i] * g_Mb[i][column];
		usleep(30*1000);	// 30 miliseconds
	}
	return value;
}

void *thread_function(void *arg) {
	int row, column, value;
	row = ((int *)arg)[0];
	column = ((int *)arg)[1];
	g_Mab[row][column] = single_multiflication(row, column);

}

int main(int argc, char **argv) {

	if (argc != 3) {
		printf("USAGE: ] ./ret <row> <column>\n");
		exit(0);
	}
	memset(g_Ma, 0, sizeof(int)*MAX_ROW*MAX_COLUMN);
	memset(g_Mb, 0, sizeof(int)*MAX_ROW*MAX_COLUMN);
	memset(g_Mab, 0, sizeof(int)*MAX_ROW*MAX_COLUMN);

	srand( time(NULL) );

	g_row = atoi(argv[1]);
	g_column = atoi(argv[2]);

	make_matrix(g_Ma, g_row, g_column);
	make_matrix(g_Mb, g_column, g_row);

	print_matrix(g_Ma, g_row, g_column);
	print_matrix(g_Mb, g_column, g_row);


	struct timeval tv1, tv2;
	gettimeofday( &tv1, NULL);
	printf("\ntime = %10d.%03d\n\n", tv1.tv_sec, tv1.tv_usec/1000);

	int res, i, j, k;
	pthread_t a_thread[MAX_THREAD];
	int arg[MAX_THREAD][2];
	void *thread_result;
	k = 0;
	for (i=0; i<g_row; i++) {
		for (j=0; j<g_row; j++) {
			arg[k][0] = i;
			arg[k][1] = j;
			res = pthread_create(&(a_thread[k]), NULL, thread_function, (void *)arg[k]);
			if (res != 0) {
				perror("Thread creation failed");
				exit(EXIT_FAILURE);
			}
			k++;
		}
	}
	
	for (i=0; i<k; i++) {
		res = pthread_join(a_thread[i], &thread_result);
		if (res != 0) {
			perror("Thread join failed");
			exit(EXIT_FAILURE);
		}
	}

	print_matrix(g_Mab, g_row, g_row);

	gettimeofday( &tv2, NULL);
	printf("\n\ntime = %10d.%03d\n\n", tv2.tv_sec, tv2.tv_usec/1000);

	if( tv2.tv_usec >= tv1.tv_usec)
		printf("Execution Time: %10d.%03d\n\n", 
		tv2.tv_sec - tv1.tv_sec, (tv2.tv_usec - tv1.tv_usec)/1000 );
	else
		printf("Execution Time: %10d.%03d\n\n", 
		tv2.tv_sec - tv1.tv_sec - 1, (1000000 + tv2.tv_usec - tv1.tv_usec)/1000 );

	printf("Execution finished successfully.... \n\n");
}
```
#### Syncronization
##### Semaphore
```c
#include <semaphore.h>

int sem_init(sem_t *sem, int pshared, unsigned int value);
int sem_wait(sem_t *sem);
int sem_post(sem_t *sem);
int sem_destory(sem_t *sem);
```
+ `sem_init()`: 주어진 `sem`을 `value`로 초기화. `pshared`는 semaphore의 type을 지정하는데, 0이면 현재 process의 local에서 사용되는 것이고, 그 외는 processes끼리 공유하는 값이다.
+ `sem_wait()`: 주어진 `sem`이 양수이면 1을 감소시킴. 아니면 양수가 될때까지 wait
+ `sem_pot()`: 주어진 `sem`을 1만큼 증가시킴
+ `sem_destroy()`: 주어진 `sem` 제거
##### Mutex
```c
#include <pthread.h>

int pthread_mutex_init(pthread_mutex_t *mutex, const pthread_mutexattr_t *mutexattr);
int pthread_mutex_lock(pthread_mutex_t *mutex);
int pthread_mutex_unlock(pthread_mutex_t *mutex);
int pthread_mutex_destroy(pthread_mutex_t *mutex);
```
##### Example
###### Semaphore1
```c
#include <stdlib.h>
#include <stdio.h>
#include <string.h>
#include <errno.h>
#include <unistd.h>
#include <math.h>
#include <fcntl.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <sys/time.h>
#include <unistd.h>
#include <signal.h>
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
#include <string.h>
#include <pthread.h>
#include <semaphore.h>

#define MAX_LOOP   3
#define MAX_THREAD 5

// An example output should be like below:
//
//	thread  0 = [  0  1  2  3  4  5  6  7  8  9 10]
//	thread  1 = [  0  1  2  3  4  5  6  7  8  9 10]
//	thread  2 = [  0  1]
//	thread  3 = [  0  1  2  3  4  5  6  7  8  9 10]
//	thread  4 = [  0  1  2  3  4  5  6  7  8  9]
//	thread  0 = [  0  1  2  3  4  5  6  7  8  9 10]
//	thread  1 = [  0  1  2  3  4  5  6  7  8  9 10]
//	thread  2 = [  0  1]
//	thread  3 = [  0  1  2  3  4  5  6  7  8  9 10]
//	thread  4 = [  0  1  2  3  4  5  6  7  8  9]
//	thread  0 = [  0  1  2  3  4  5  6  7  8  9 10]
//	thread  1 = [  0  1  2  3  4  5  6  7  8  9 10]
//	thread  2 = [  0  1]
//	thread  3 = [  0  1  2  3  4  5  6  7  8  9 10]
//	thread  4 = [  0  1  2  3  4  5  6  7  8  9]

sem_t g_sem_list[MAX_THREAD];
sem_t bin_sem;

void print_int_list(int id, int max_num) {
	printf("thread %2d = [", id); fflush(stdout);
	
	for(int i=0; i<=max_num; i++) {
		printf("%3d", i); fflush(stdout);
		usleep(100*1000);	// sleep 0.1 second
	}
	printf("]\n");
}

void *thread_function(void *arg) {
	int id = *(int*)arg;
	int max_num  = rand() % 10 + 1;	

	for(int i=0; i<MAX_LOOP; i++) {	
		sem_wait(&(g_sem_list[id]));
		print_int_list(id, max_num);
		sem_post(&(g_sem_list[(id+1)%MAX_THREAD]));
	}

    pthread_exit(NULL);
}

int main(int argc, char **argv) {
	if (argc != 1) {
		printf("USAGE: ] ./ret\n");
		exit(0);
	}

	srand( time(NULL) );

	int res, i;
    pthread_t thread_id[MAX_THREAD];
	int thread_arg[MAX_THREAD];

    res = sem_init(&bin_sem, 0, 1);
    if (res != 0) {
        perror("Semaphore initialization failed");
        exit(EXIT_FAILURE);
    }
	for (i=0; i<MAX_THREAD; i++)
		sem_init(&(g_sem_list[i]), 0, 0);

	sem_post(&(g_sem_list[0]));
	for( i=0; i<MAX_THREAD; i++) {
		thread_arg[i] = i;
		//printf("arg = %2d\n", thread_arg[i]);
		res = pthread_create(&thread_id[i], NULL, thread_function, (void *)&thread_arg[i]);
	}

	for( i=0; i<MAX_THREAD; i++) {
		res = pthread_join(thread_id[i], NULL);
		//printf("Thread %d finished ...\n", i);
	}

    sem_destroy(&bin_sem);
	for (i=0; i<MAX_THREAD; i++)
		sem_destroy(&(g_sem_list[i]));
		
	printf("Execution finished successfully.... \n\n");
}

```
###### Semaphore2
```c
#include <stdlib.h>
#include <stdio.h>
#include <string.h>
#include <errno.h>
#include <unistd.h>
#include <math.h>
#include <fcntl.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <sys/time.h>
#include <unistd.h>
#include <signal.h>
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
#include <string.h>
#include <pthread.h>
#include <semaphore.h>

#define MAX_THREAD 5
#define MAX_LOOP   5

//thread  0 = [  0  1]
//thread  1 = [  0  1]
//thread  2 = [  0  1  2  3  4  5]
//thread  3 = [  0  1  2  3  4  5  6]
//thread  4 = [  0  1  2  3  4  5  6  7]
//thread  4 = [  0  1  2  3  4  5  6  7]
//thread  3 = [  0  1  2  3  4  5  6]
//thread  2 = [  0  1  2  3  4  5]
//thread  1 = [  0  1]
//thread  0 = [  0  1]
//thread  0 = [  0  1]
//thread  1 = [  0  1]
//thread  2 = [  0  1  2  3  4  5]
//thread  3 = [  0  1  2  3  4  5  6]
//thread  4 = [  0  1  2  3  4  5  6  7]
//thread  4 = [  0  1  2  3  4  5  6  7]
//thread  3 = [  0  1  2  3  4  5  6]
//thread  2 = [  0  1  2  3  4  5]
//thread  1 = [  0  1]
//thread  0 = [  0  1]
//thread  0 = [  0  1]
//thread  1 = [  0  1]
//thread  2 = [  0  1  2  3  4  5]
//thread  3 = [  0  1  2  3  4  5  6]
//thread  4 = [  0  1  2  3  4  5  6  7]
//Execution finished successfully.... 

sem_t bin_sem;
sem_t g_sem_list[MAX_THREAD];
int g_flag  = 0;
int g_index = 0;

void print_int_list(int id, int max_num) {
	printf("thread %2d = [", id); fflush(stdout);
	
	for(int i=0; i<=max_num; i++)
	{
		printf("%3d", i); fflush(stdout);
		usleep(100*1000);	// sleep 0.1 second
	}
	printf("]\n");
}

void *thread_function(void *arg) {

	int id = *(int*)arg;
	int max_num  = rand() % 10 + 1;	

	for(int i=0; i<MAX_LOOP; i++) {
		sem_wait(&(g_sem_list[id]));
		print_int_list(id, max_num);
		
		sem_wait(&bin_sem);
		g_index++;
		if (g_index == MAX_THREAD) {
			g_flag = g_flag ^ 1;
			g_index = 0;
			sem_post(&(g_sem_list[id]));
		}
		else {
			if (g_flag == 0)
				sem_post(&(g_sem_list[(id+1)%MAX_THREAD]));
			else 
				sem_post(&(g_sem_list[(id-1)%MAX_THREAD]));
		}
		sem_post(&bin_sem);
	}
    pthread_exit(NULL);
}

int main(int argc, char **argv) {
	if (argc != 1) 	{
		printf("USAGE: ] ./ret\n");
		exit(0);
	}

	srand( time(NULL) );

	int res, i;
    pthread_t thread_id[MAX_THREAD];
	int thread_arg[MAX_THREAD];

	for (i=0; i<MAX_THREAD; i++)
		sem_init(&(g_sem_list[i]), 0, 0);
	sem_init(&bin_sem, 0, 1);

	sem_post(&(g_sem_list[0]));
	
	for( i=0; i<MAX_THREAD; i++) {
		thread_arg[i] = i;
		//printf("arg = %2d\n", thread_arg[i]);
		res = pthread_create(&thread_id[i], NULL, thread_function, (void *)&thread_arg[i]);
	}
	

	for( i=0; i<MAX_THREAD; i++) {
		res = pthread_join(thread_id[i], NULL);
		//printf("Thread %d finished ...\n", i);
	}

	for (i=0; i<MAX_THREAD; i++)
		sem_destroy(&(g_sem_list[i]));
	sem_destroy(&bin_sem);

	printf("Execution finished successfully.... \n\n");
}
```
###### Mutex
```c
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
#include <string.h>
#include <pthread.h>
#include <semaphore.h>
#define WORK_SIZE 1024

pthread_mutex_t work_mutex; /* protects both work_area and time_to_exit */
char work_area[WORK_SIZE];
int time_to_exit = 0;

void *thread_function(void *arg) {
    sleep(1);
    pthread_mutex_lock(&work_mutex);
    while(strncmp("end", work_area, 3) != 0) {
        printf("You input %d characters\n", strlen(work_area) -1);
        work_area[0] = '\0';
        pthread_mutex_unlock(&work_mutex);
        sleep(1);
        pthread_mutex_lock(&work_mutex);
        while (work_area[0] == '\0' ) {
            pthread_mutex_unlock(&work_mutex);
            sleep(1);
            pthread_mutex_lock(&work_mutex);
        }
    }
    time_to_exit = 1;
    work_area[0] = '\0';
    pthread_mutex_unlock(&work_mutex);
    pthread_exit(0);
}

int main() {
    int res;
    pthread_t a_thread;
    void *thread_result;
    res = pthread_mutex_init(&work_mutex, NULL);
    if (res != 0) {
        perror("Mutex initialization failed");
        exit(EXIT_FAILURE);
    }
    res = pthread_create(&a_thread, NULL, thread_function, NULL);
    if (res != 0) {
        perror("Thread creation failed");
        exit(EXIT_FAILURE);
    }
    pthread_mutex_lock(&work_mutex);
    printf("Input some text. Enter 'end' to finish\n");
    while(!time_to_exit) {
        fgets(work_area, WORK_SIZE, stdin);
        pthread_mutex_unlock(&work_mutex);
        while(1) {
            pthread_mutex_lock(&work_mutex);
            if (work_area[0] != '\0') {
                pthread_mutex_unlock(&work_mutex);
                sleep(1);
            }
            else {
                break;
            }
        }
    }
    pthread_mutex_unlock(&work_mutex);
    printf("\nWaiting for thread to finish...\n");
    res = pthread_join(a_thread, &thread_result);
    if (res != 0) {
        perror("Thread join failed");
        exit(EXIT_FAILURE);
    }
    printf("Thread joined\n");
    pthread_mutex_destroy(&work_mutex);
    exit(EXIT_SUCCESS);
}
```