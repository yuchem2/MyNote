[[운영체제/Process|Process]] 참고 
#### Shell command
##### ps 
```shell
$ ps -ef
UID    PID   PPID   C  STIME  TTY   TIME       CMD
rick   101   96     0  18:24  tty2  00:00:00   grep troi nextgen.doc
neil   102   92     0  18:24  tty4  00:00:00   grep kirk trek.txt
```
+ PID: 각 프로세스에 부여되는 식별되는 번호. 2~32,768 값을 가짐
+ PPID: 그 프로세스의 부모 프로세스 번호
+ TTY: 어떤 터미널에서 그 프로세스가 시작되었는지 표시
+ TIME: 현재 사용한 CPU time
+ CMD: 그 프로세스가 실행 중인 command

```shell
$ ps ax
  PID  TTY    STAT  TIME  COMMAND
    1  ?      Ss    0:03  init [5]
    2  ?      S     0:00  [migration/0]
    3  ?      SN    0:00  [ksoftirqd/0]
    4  ?      S<    0:05  [events/0]
    5  ?      S<    0:00  [khelper]
    6  ?      S<    0:00  [kthread]
  840  ?      S<    2:52  [kjournald]
  888  ?      S<s   0:03  /sbin/udevd --daemon
 3069  ?      Ss    0:00  /sbin/acpid
 3098  ?      Ss    0:11  /usr/sbin/hald --daemon=yes
 3099  ?      S     0:00  hald-runner
 8357  ?      Ss    0:03  /sbin/syslog-ng
 8677  ?      Ss    0:00  /opt/kde3/bin/kdm
 9119  ?      S     0:11  /konsole [kdeinit]
 9120  pts/2  Ss    0:00  /bin/bash
 9151  ?      Ss    0:00  /usr/sbin/cupsd
 9457  ?      Ss    0:00  /usr/sbin/cron
 9479  ?      Ss    0:00  /usr/sbin/sshd -o PidFile=/var/run/sshd.init.pid
21198  ?      S     0:00  pickup -l -t fifo -u
21475  pts/2  R+    0:00  ps ax
```
+ STAT code
	+ `S`: sleeping
	+ `R`: running
	+ `D`: uninterruptible sleep
	+ `T`: stopped
	+ `Z`: zombie process
	+ `N`: low prioirt task, nice
	+ `W`: paging
	+ `s`: session leader
	+ `+`: foreground process group
	+ `l`: process is multithreaded
	+ `<`: high priority task
##### nice
```shell
$ nice oclock &
$ renice 10 1362
1362: old priority 0, new priority 10
```
+ nice 값을 10 증가 시킨다. $\rightarrow$ 그 프로세스의 우선순위를 10정도 낮춘다. 
+ 우선순위는 -20~20 값을 가지며 작을 수록 높은 우선순위를 가진다.
+ `ps -l` 명령어를 통해 nice 값을 확인할 수 있다. 
```shell
$ ps -l
  F S  UID   PID  PPID  C  PRI  NI ADDR SZ WCHAN  TTY         TIME CMD
000 S  500  1259  1254  0   75   0 -   710 wait4  pts/2   00:00:00 bash
000 S  500  1262  1251  0   75   0 -   714 wait4  pts/1   00:00:00 bash
000 S  500  1313  1262  0   75   0 -  2762 schedu pts/1   00:00:00 emacs
000 S  500  1362  1262  0   90  10 -   789 schedu pts/1   00:00:00 oclock
000 R  500  1365  1262  0   81   0 -   782 -      pts/1   00:00:00 ps
```
#### Starting new process
##### Create new process
```c
#include <stdlib.h>

int system(const char *string);
```
+ shell command를 실행시켜준다.
+ `return` 값은 127(command 실행 불가), -1(다른 에러 발생), command의 exit code(그 외)
##### Replace process image
```c
#include <unistd.h>

char **environ;

int execl(const char *path, const char *arg0, ..., (char *)0);
int execlp(const char *file, const char *arg0, ..., (char *)0);
int execle(const char *file, const char *arg0, ..., (char *)0, char *const envp[]);
int execv(const char *path, char *const argv[]);
int execvp(const char *file, char *const argv[]);
int execve(const char *path, char *const argv[], char *const envp[]);
```
+ `exec()`: 현재 프로세스를 입력된 경로 혹은 파일로부터 새로운 프로세스를 만들어 대치
##### Duplicating process image
```c
#include <sys/types.h>
#include <unistd.h>

pid_t fork(void);
```
+ `fork()`: 새로운 프로세스를 실행한다. 이때 현재 호출된 프로세스를 완전히 복제하여 수행된다. 
+ `return` 값은 0(자식 프로세스), -1(error), 이외 값(부모 프로세스)
##### Waiting for a process
```c
#include <sys/types.h>
#include <sys/wait.h>

pid_t wait(int *stat_loc);
pid_t waitpid(pid_t pid, int *stat_loc, int options);
```
+ `wait()`: 부모 프로세스가 자식 프로세스 중 하나가 종료될 때까지 기다린다. 
	+ `return` 값은 자식 프로세스의 PID
+ `waitpid()`: 주어진 프로세스가 종료될 때까지 기다림
	+ `options`을 통해 다양한 옵션 제공. 그 중 많이 사용되는 것은 `WNOHANG`으로, 대기 상태를 방지하고, 단순히 종료/동작 여부를 확인할 수 있도록 하는 옵션. 기본 값은 0이다.

+ `stat_loc` 값은 자식 프로세스의 상태 정보를 가지고 있다. 다음 macro를 이용해 값을 정보를 얻을 수 있다.
	+ `WIFEXITED(stat_val)`: 자식 프로세스가 일반 종료되면 nonzero
	+ `WEXITSTATUS(stat_val)`: `WIFEXITED`가 nonzero이면, 자식 프로세스의 종료코드를 리턴
	+ `WIFSIGNALED(stat_val)`: 자식 프로세스가 예상치 못한 signal로 종료되면 nonzero
	+ `WTERMSIG(stat_val)`: `WIFSIGNALED`가 nonzero이면, signal 번호 리턴
	+ `WIFSTOPPED(stat_val)`: 자식 프로세스가 멈추면 nonzero
	+ `WSTOPSIG(stat_val)`: `WIFSTOPPED`가 nonzero이면, singal 번호 리
##### Example
###### exec()
```c
#include <unistd.h>

int main() {
	char *const ps_argv[] = {"ps", "ax", 0};
	char *const ps_envp[] = {"PATH=/bin:/usr/bin", "TERM=console", 0};

	execl("/bin/ps", "ps", "ax", 0);
	execlp("ps", "ps", "ax", 0);
	execle("/bin/ps", "ps", "ax", 0, ps_envp);

	execv("/bin/ps", ps_argv);
	execvp("ps", ps_argv);
	execve("/bin/ps", ps_argv, ps_envp);
}
```
###### fork()
```c
#include <sys/types.h>
#include <unistd.h>
#include <stdio.h>
#include <stdlib.h>

int main() {
    pid_t pid;
    char *message;
    int n;

    printf("fork program starting\n");
    pid = fork();
    switch(pid) {
    case -1:
        perror("fork failed");
        exit(1);
    case 0:
        message = "This is the child";
        n = 5;
        break;
    default:
        message = "This is the parent";
        n = 3;
        break;
    }

    for(; n > 0; n--) {
        puts(message);
        sleep(1);
    }
    exit(0);
}
```
###### wait()
```c
#include <sys/types.h>
#include <sys/wait.h>
#include <unistd.h>
#include <stdio.h>
#include <stdlib.h>

int main() {
    pid_t pid;
    char *message;
    int n;
    int exit_code;

    printf("fork program starting\n");
    pid = fork();
    switch(pid) {
    case -1:
        exit(1);
    case 0:
        message = "This is the child";
        n = 5;
        exit_code = 37;
        break;
    default:
        message = "This is the parent";
        n = 3;
        exit_code = 0;
        break;
    }

    for(; n > 0; n--) {
        puts(message);
        sleep(1);
    }

/*  This section of the program waits for the child process to finish.  */

    if(pid) {
        int stat_val;
        pid_t child_pid;

        child_pid = wait(&stat_val);

        printf("Child has finished: PID = %d\n", child_pid);
        if(WIFEXITED(stat_val))
            printf("Child exited with code %d\n", WEXITSTATUS(stat_val));
        else
            printf("Child terminated abnormally\n");
    }
    exit (exit_code);
}
```
###### Execute multiple processes
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
#include <sys/wait.h>

// For example, when you execute this program with the command line "]$./ret /etc 'httpd*' '*xml' 'web*'
// This program executes three "find" programs concurrently
//	] find /etc -name 'httpd*' 
//	] find /etc -name '*xml'
//  ] find /etc -name 'web*'

#define MAX_CHILD_MAX	100
int main(int argc, char **argv) {
	int i;
	if (argc < 3) 	{
		printf("USAGE  : ] ./ret [dir_path] [search pattern list]\n");
		printf("Example: ] ./ret /home  ret.c Makefile\n");
		printf("Example: ] ./ret /etc  'httpd*' '*xml' 'web*'\n");

		exit(0);
	}

	for(i=2; i<argc; i++)
		printf("search word %2d = [%s]\n", i, argv[i]);
	printf("\n");

	// 입력된 각 패턴을 독립된 프로세스로 동시에 실행시키기...

	pid_t pid;
	for(i=2; i<argc; i++) {
		pid = fork();
		if (pid == 0) {
			execlp("find", "find", argv[1], "-name", argv[i], 0);
			break;
		}
		else if(pid < 0) {
			printf("fork error\n");
			return 1;
		}
		else {
			printf("child process %d: find %s -name %s\n", pid, argv[1], argv[i]);
		}
	}
	
	for(i=2; i<argc; i++)
		wait(NULL);

	if (pid) {
		printf("\nAll child processes finished successfully.... \n\n");
	}
	return 1;
}

```
###### Count the number of process
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
#include <dirent.h>

// ]$ ls /proc | grep ^[0-9] | wc | awk '{print $1}'
// ]$ ps ax | wc | awk '{print $1}'
// ]$ watch -n 1 'ls /proc | grep ^[0-9] | wc -l'
// ]$ watch -n 1 'ps ax | wc -l'

int process_count()
{
	int count = 0;

	// proc 디렉토리에 저장되어 있는 중 숫자로 시작하는 모든 파일의 개수를 세기
	DIR *dp;
	struct dirent *cnt;
	struct stat statbuf;
	
	dp = opendir("/proc");
	chdir("/proc");

	while ((cnt = readdir(dp)) != NULL) {
		lstat(cnt->d_name, &statbuf);	// read file status
		// check if the file is a directory
		if(S_ISDIR(statbuf.st_mode)) {
			if(cnt->d_name[0] >= '0' && cnt->d_name[0] <= '9')
				count++;
		}
	}

	return count;
}

int main(int argc, char **argv) {
	int count = 0, i;

	if (argc != 1) {
		printf("USAGE: ] ret \n");
		exit(0);
	}
	
	for( i=0; i<10; i++) {
		printf("Process Count[%3d] : %3d\n", count++, process_count());
		sleep(1);
	}
}
```
