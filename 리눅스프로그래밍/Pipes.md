Pipe란 하나의 프로세스에서 다른 프로세스를 연결하는 데이터 flow이다. 이러한 간단한 예시는 `ls -l | wc`와 같은 명령어로 `ls -l`의 출력이 `wc`의 입력이 되는 형태이다.
#### Process Pipes
##### Simplest Way
```c
#include <stdio.h>

FILE *popen(const char *command, const char *open_mode);
int pclose(FILE *stream_to_close);
```
+ `popen()`: 다른 프로그램을 새로운 프로세스처럼 만들어 그 프로그램에 데이터를 입력하거나 전송할 수 있는 상태로 만든다. `open_mode`는 `r`, `w`가 존재한다.
+ `pclose()`: 열린 file stream을 종료시킨다.
###### Examples
```c
#include <unistd.h>
#include <stdlib.h>
#include <stdio.h>
#include <string.h>

int main() {
    FILE *read_fp;
    char buffer[BUFSIZ + 1];
    int chars_read;
    memset(buffer, '\0', sizeof(buffer));
    read_fp = popen("uname -a", "r");
    if (read_fp != NULL) {
        chars_read = fread(buffer, sizeof(char), BUFSIZ, read_fp);
        if (chars_read > 0) {
            printf("Output was:-\n%s\n", buffer);
        }
        pclose(read_fp);
        exit(EXIT_SUCCESS);
    }
    exit(EXIT_FAILURE);
}
```

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
#include <unistd.h>
#include <stdlib.h>
#include <stdio.h>
#include <string.h>

// Complete a program which executes the following shell commands.
// ] ls -l | wc
// 1. Implement it by using two popen() pipe functions.
// 2. execute each command by using seperate popen(). 
//		a popen() for 'ls' command, the other popen() for 'wc' command.

int main(int argc, char **argv) {
   	int res, i;
	if (argc != 1) 	{
		printf("USAGE: ] ./ret \n");
		exit(EXIT_FAILURE);
	}

    FILE *read_fp, *write_fp;
    char buffer[BUFSIZ + 1];
    int chars_read;

    memset(buffer, '\0', sizeof(buffer));

	read_fp = popen("ls -l", "r");
	write_fp = popen("wc", "w");
	if (read_fp != NULL && write_fp != NULL) {
		do {
			chars_read = fread(buffer, sizeof(char), BUFSIZ, read_fp);
			fwrite(buffer, sizeof(char), chars_read, write_fp);
		}
		while (chars_read);

		pclose(write_fp);
		pclose(read_fp);
	}

	printf("Execution finished successfully.... \n\n");
    exit(EXIT_SUCCESS);

}
```

##### Low-level pipe function
```c
#include <unistd.h>

int pipe(int file_descriptor[2]);
```
+ `pipe()`는 두 개의 file descriptor를 받는다. `file_descriptor[0]`은 출력 정보를, `file_descriptor[1]`은 입력 정보를 받는다.
###### Example
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
#include <unistd.h>
#include <stdlib.h>
#include <stdio.h>
#include <string.h>

// Implement the following shell command using pipe() functions
// ] ls -l | wc -lmc 

int main(int argc, char **argv) {
   	int res, i;
	if (argc != 1) 	{
		printf("USAGE: ] ./ret \n");
		exit(EXIT_FAILURE);
	}

    int file_pipes[2];
    pid_t fork_result;

	if (pipe(file_pipes) == 0) {
		fork_result = fork();
		if (fork_result == -1) {
			fprintf(stderr, "Fork failure");
			exit(EXIT_FAILURE);
		}
		
		if(fork_result == 0) {
			close(0); // stdin close
			dup(file_pipes[0]); // file_pipes[0] to stdin
			close(file_pipes[0]);
			close(file_pipes[1]);
			execlp("wc", "wc", "-lmc", (char *)0);
		}
		else {
			close(1); // stdout close
			dup(file_pipes[1]); // file_pipes[1] to stdout
			execlp("ls", "ls", "-l", (char *)0);
		}
	}

	printf("Execution finished successfully.... \n\n");
    exit(EXIT_SUCCESS);
}
```
##### Named Pipes: FIFOs
###### Main
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
#include <unistd.h>
#include <stdlib.h>
#include <stdio.h>
#include <string.h>
#include <sys/types.h>
#include <sys/wait.h>

// Complete this server program which acts as follows
// - receive a string from a client through a named pipe [./my_fifo] 
// - print the string on the screen
// - convert the string to UPPER case.
// - print the result string on the screen.
// - send the result string to the client through a named pipe [./my_fifo]

/*
step 1. [client] ---> "abc" ---> [server]
step 2. [client] <--- "ABC" <--- [server]
*/
int main(int argc, char **argv) {
   	int res, i;
	if (argc != 2) 	{
		printf("USAGE: ] ./ret [string] \n");
		exit(EXIT_FAILURE);
	}

    pid_t fork_result;

	// execute server
	fork_result = fork();
	if (fork_result == (pid_t)-1) {
		fprintf(stderr, "Fork failure");
		exit(EXIT_FAILURE);
	}

	if (fork_result == (pid_t)0) {
		execl("./server", "server", (char *)0);
		exit(EXIT_FAILURE);
	}

	usleep(100000);		// 100 msec of sleep

	// execute client
	fork_result = fork();
	if (fork_result == (pid_t)-1) {
		fprintf(stderr, "Fork failure");
		exit(EXIT_FAILURE);
	}

	if (fork_result == (pid_t)0) {
		execl("./client", "client", argv[1], (char *)0);
		exit(EXIT_FAILURE);
	}

	// wait for the processes to finish
	int status;
	wait(&status);
	wait(&status);

	printf("\n\n");
	printf("Execution finished successfully.... ./ret\n\n");
    exit(EXIT_SUCCESS);
}
```

###### Client
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
#include <unistd.h>
#include <stdlib.h>
#include <stdio.h>
#include <string.h>

#define FIFO_NAME "./my_fifo"
#define BUFFER_SIZE 100

int main(int argc, char **argv) {
    int pipe_fd;
    int res=0, i;
    int open_mode = O_WRONLY;
    char buffer[BUFFER_SIZE];

	if (argc != 2) 	{
		printf("USAGE: ] ./client [string] \n");
		exit(0);
	}

	strcpy( buffer, argv[1]);
    printf("Client Input  Data: %s\n", buffer); //exit(0);


	pipe_fd = open(FIFO_NAME, open_mode);
	if (pipe_fd == -1) {
		fprintf(stderr, "No server\n");
		exit(EXIT_FAILURE);
	}
	
	write(pipe_fd, buffer, BUFFER_SIZE);
	close(pipe_fd);

	pipe_fd = open(FIFO_NAME, O_RDONLY);
	res = read(pipe_fd, buffer, BUFFER_SIZE);

	
	printf("Client Output Data: %s\n", buffer);

	sleep(1);
	printf("\n\n");
    printf("Client Process %d finished\n", getpid());
	printf("Client Execution finished successfully.... \n");
    exit(EXIT_SUCCESS);
}

```
###### Sever
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
#include <unistd.h>
#include <stdlib.h>
#include <stdio.h>
#include <string.h>

#define FIFO_NAME "./my_fifo"
#define BUFFER_SIZE 100

int main(int argc, char **argv)
{
	int i;
    int pipe_fd;
    int res=0;
    char buffer[BUFFER_SIZE];
	if (argc != 1)
	{
		printf("USAGE: ] ./server \n");
		exit(0);
	}

    memset(buffer, '\0', sizeof(buffer));
    
	// write your codes here only
	//----------------->
	if(access(FIFO_NAME, F_OK) == -1) {
		mkfifo(FIFO_NAME, 0777);
	}

	pipe_fd = open(FIFO_NAME, O_RDONLY);
	if (pipe_fd == -1) {
		fprintf(stderr, "Server fifo failure\n");
		exit(EXIT_FAILURE);
	}
	
	res = read(pipe_fd, buffer, BUFFER_SIZE);
	printf("Server Input  Data: %s\n", buffer); 
	for(i=0; i<res; i++) buffer[i] = toupper(buffer[i]);
    printf("Server Output Data: %s\n", buffer); 

	close(pipe_fd);
	pipe_fd = open(FIFO_NAME, O_WRONLY);
	write(pipe_fd, buffer, BUFFER_SIZE);
	close(pipe_fd);
	unlink(FIFO_NAME);

	sleep(1);
	printf("\n\n");
	printf("Server Process %d finished\n", getpid());
  	printf("Server Execution finished successfully.... \n");
    exit(EXIT_SUCCESS);
}
```
