Linux system에서 특정 조건에 반응하기 위해 생성되는 event이다. 이렇게 생성된 signal은 프로세스에게 전달되고, 그 프로세스는 signal에 따라 특정 행동을 취한다. 

Signal에 대해 다음과 같은 행동을 취할 수 있다. 
+ Raised: signal 생성
+ Caught: signal을 받음
+ Acted: signal에 따른 행동 수행
+ Ignored: signal 무시
#### Signals
| Signal name | number | Default Action        | Description                                          |
| ----------- | ------ | --------------------- | ---------------------------------------------------- |
| `SIGILL`    | 4      | Terminate + Core Dump | Illegal instruction                                  |
| `SIGFPE`    | 8      | Terminate + Core Dump | Floating-point exception                             |
| `SIGSEGV`   | 11     | Terminate + Core Dump | Invalid Memony Segmanet violation                    |
| `SIGPIPE`   | 13     | Terminate             | When a process tries to write on PIPE with no reader |
| `SIGHUP`    | 1      | Terminate             | When a terminal desconnects with fg process          |
| `SIGINT`    | 2      | Terminate             | `Ctrl+C`, Terminal interrupt                         |
| `SIGTSTP`   | 20     | SUSPENDED             | `Ctrl+Z`, Terminal stop signal                       |
| `SIGQUIT`   | 3      | Terminate + Core Dump | `Ctrl+\`, Terminal quit                              |
| `SIGCHLD`   | 17     | IGNORED               | When a child process terminates(stopped or exited)   |
| `SIGALARM`  | 14     | Terminate             | `alarm()`, Alarm clock                               |
| `SIGABRT`   | 6      | Terminate             | `kill()`, Process abort                              |
| `SIGKILL`   | 9      | Terminate             | `kill()`, Can't be caught for user defined function  |
| `SIGUSR1`   | 10     | Terminate             | `kill()`, User-defined signal 1                      |
| `SIGUSR2`   | 12     | Terminate             | `kill()`, User-defined signal 1                      |
| `SIGTERM`   | 15     | Terminate             | `kill()`, Shutdown command sends this to all daemons |
| `SIGCONT`   | 18     | CONTINUED             | `kill()`, Continued executing, if stopped            |
| `SIGSTOP`   | 19     | SUSPENDED             | `kill()`, Stop executing(Can't be caught or ignored) |
| `SIGTTIN`   | 21     | SUSPENDED             | When a big program tries to read from terminal       |
| `SIGTTOU`   | 22     | SUSPENDED             | When a big program tries to write to terminal        |

```shell
$ kill -l
 1) SIGHUP	     2) SIGINT	     3) SIGQUIT	     4) SIGILL	     5) SIGTRAP
 6) SIGABRT	     7) SIGBUS	     8) SIGFPE	     9) SIGKILL	    10) SIGUSR1
11) SIGSEGV	    12) SIGUSR2	    13) SIGPIPE	    14) SIGALRM	    15) SIGTERM
16) SIGSTKFLT	17) SIGCHLD	    18) SIGCONT	    19) SIGSTOP	    20) SIGTSTP
21) SIGTTIN	    22) SIGTTOU	    23) SIGURG	    24) SIGXCPU	    25) SIGXFSZ
26) SIGVTALRM	27) SIGPROF	    28) SIGWINCH	29) SIGIO	    30) SIGPWR
31) SIGSYS	    34) SIGRTMIN	35) SIGRTMIN+1	36) SIGRTMIN+2	37) SIGRTMIN+3
38) SIGRTMIN+4	39) SIGRTMIN+5	40) SIGRTMIN+6	41) SIGRTMIN+7	42) SIGRTMIN+8
43) SIGRTMIN+9	44) SIGRTMIN+10	45) SIGRTMIN+11	46) SIGRTMIN+12	47) SIGRTMIN+13
48) SIGRTMIN+14	49) SIGRTMIN+15	50) SIGRTMAX-14	51) SIGRTMAX-13	52) SIGRTMAX-12
53) SIGRTMAX-11	54) SIGRTMAX-10	55) SIGRTMAX-9	56) SIGRTMAX-8	57) SIGRTMAX-7
58) SIGRTMAX-6	59) SIGRTMAX-5	60) SIGRTMAX-4	61) SIGRTMAX-3	62) SIGRTMAX-2
63) SIGRTMAX-1	64) SIGRTMAX	
```

#### Handling Signals
##### signal()
```c
#include <signal.h>

void (*signal(int sig, void (*func)(int)))(int);
```
+ `sig`는 caught or ignored 할 signal 번호를 의미.
+ `func`는 `sig`가 전달되었을 때 수행할 함수를 의미. 이때 `SIG_IGN`(signal 무시), `SIG_DFL`(기본 행동 수행)로 대치될 수 있다.
##### Sending signals
```c
#include <sys/types.h>
#include <singal.h>
#include <unistd.h>

int kill(pid_t pid, int sig);
unsigned int alarm(unsigned int seconds);
int pause(void);
```
+ `kill()`: 주어진 프로세스에게 `sig`를 전달
+ `alarm()`: 호출된 시점으로부터 `seconds` 초가 지나면 `SIGALRM` signal을 전달한다. 
+ `pause()`: 호출된 프로세스를 일시정지시킨다. 
##### Example
###### Signal()
```c
#include <signal.h>
#include <stdio.h>
#include <unistd.h>

void ouch(int sig) {
    printf("OUCH! - I got signal %d\n", sig);
    (void) signal(SIGINT, SIG_DFL);
}

int main() {
    (void) signal(SIGINT, ouch);
    while(1) {
        printf("Hello World!\n");
        sleep(1);
    }
}
```
###### Alarm()
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

// The expiration time is given to the program as an argument in seconds.
// You can use the signal SIGALRM.

int g_i=0;

void ding(int sg) {
	g_i = -1;
}

int main(int argc, char **argv) {
	if (argc != 2) 	{
		printf("USAGE: ] ./ret  <seconds>\n");
		exit(0);
	}
	
	struct timeval tv1, tv2;
	gettimeofday( &tv1, NULL);
	printf("\ntime = %10d.%03d\n\n", tv1.tv_sec, tv1.tv_usec/1000);

	alarm(atoi(argv[1]));
	signal(SIGALRM, ding);

	for(g_i=1; g_i ; g_i++) {
		printf("%3d ", g_i); fflush(stdout);	
		if( g_i % 20 == 0 ) printf("\n");
		usleep(100000);		// 0.1초 = 1 
	}
	gettimeofday( &tv2, NULL);
	printf("\n\ntime = %10d.%03d\n\n", tv2.tv_sec, tv2.tv_usec/1000);

	if( tv2.tv_usec >= tv1.tv_usec)
		printf("Execution Time: %10d:%03d\n\n", tv2.tv_sec - tv1.tv_sec, (tv2.tv_usec - tv2.tv_usec)/1000 );
	else
		printf("Execution Time: %10d:%03d\n\n", tv2.tv_sec - tv1.tv_sec - 1, (1000000 + tv2.tv_usec - tv2.tv_usec)/1000 );
}
```
###### fork() and signal() 
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

// Complete the program which control the execution of a chile process.
// ]$ ./ret  <pause> <resume> <finish> 
// ]$ ./ret  3 5 10 
//	pause = pause the child process execution after 3 seconds
//  resume = resume the execution after 5 seconds
//	finish = finish the execution after 10 seconds
void chile_process_exe() {
	int i, j;
	time_t init_time = time(NULL);
	for (j=0; j<20 ; j++ ) 	{
		printf("Chile Process = %2d %2d    ", j, time(NULL)-init_time);
		for(i=0; i<10; i++) {
			printf("%d ", i); fflush(stdout);	
			usleep(100000);		// 0.1 second sleep
		}
		printf("\n");
	}
}

int main(int argc, char **argv) {
	if (argc != 4) {
		printf("USAGE: ] ./ret  <pause> <resume> <finish>\n");
		exit(0);
	}
	
	struct timeval tv1, tv2;
	gettimeofday( &tv1, NULL);
	printf("\ntime = %10d.%03d\n\n", tv1.tv_sec, tv1.tv_usec/1000);
	
    pid_t pid;
	int pause, resume, finish;

    pid = fork();
    switch(pid) {
    case -1:
      perror("fork failed");
      exit(1);
    case 0:
        chile_process_exe();
		exit(0);
	default:
		sleep(atoi(argv[1]));
		kill(pid, SIGSTOP);
		sleep(atoi(argv[2]) - atoi(argv[1]));
		kill(pid, SIGCONT);
		sleep(atoi(argv[3]) - atoi(argv[2]));
		kill(pid, SIGTERM);
		wait();		
	}

	gettimeofday( &tv2, NULL);
	printf("\n\ntime = %10d.%03d\n\n", tv2.tv_sec, tv2.tv_usec/1000);

	if( tv2.tv_usec >= tv1.tv_usec)
		printf("Execution Time: %10d.%03d\n\n", 
		tv2.tv_sec - tv1.tv_sec, (tv2.tv_usec - tv1.tv_usec)/1000 );
	else
		printf("Execution Time: %10d.%03d\n\n", 
		tv2.tv_sec - tv1.tv_sec - 1, (1000000 + tv2.tv_usec - tv1.tv_usec)/1000 );
}
```