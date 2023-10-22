### Use File
#### File concepts
Linux에서는 모든 것이 파일로서 다루어진다. Device들도 모두 하나의 파일로서 다루어 진다. 파일을 다루는 기본적인 명령어는 다음과 같다.
+ open
+ close
+ read
+ write
+ ioctl

파일은 이름과 특성을 가지고 있는데, 이러한 정보들은 모두 각 파일의 inode에 저장되어 있다. inode를 확인하기 위해서는 다음과 같은 명령어를 사용 `ls -i <file>` or `stat <file>`

각 폴더는 속한 파일들의 inode 번호와 이름을 정보로 가지고 있다.

Device files은 모두 `/dev`에 위치하고 있으며 다음과 같은 예시들이 존재한다. 
+ `/dev/console`: system console이 위치하고 있는 곳으로, 오직 하나의 장치만 존재. 
+ `/dev/tty`: teletypewrite(=terminal)를 의미. 관련된 명령어는 다음과 같음
	+ `tty`: 현재 terminal의 이름 출력
	+ `who`: 로그인한 유저들 출
+ `/dev/null`: 모든 출력이 사라지는 디바이스. 관련된 명령어는 다음과 같음
	+ `cp /dev/null a` == `touch a`
	+ `find . -name *.c -print > /dev/null`: 결과값을 출력하지 않음
+ `/dev/zero`: 0x00을 출력하는 디바이스 사용하는 방법은 다음과 같음
	+ `dd if=/dev/zero of=a ibs=1k count=10`: 1KB의 길이의 0을 10번 a파일에 작성

#### low-level File Access
+ Standard I/O and error are descripted by number `0, 1, 2`
+ Others are descripted by each name
##### write
```c
#include <unistd.h>

size_t write(int fildes, const void *buf, size_t nbytes);
```
+ `buf`로 부터 첫 `nbytes`를 읽어 `fildes`에 기록한다
+ `return` 값은 기록한 바이트 수이고, 만약 기록에 오류가 생긴 경우 `-1`을 리턴한다.

```c 
#include <unistd.h>
#include <stdlib.h>

int main() {
	if ((write(1, "Here is some data\n", 18)) != 18)
		write(2, "A write error has occurred on file descriptor 1\n", 46);
	exit(0);
}
```
##### read
```c
#include <unistd.h>

size_t read(int fildes, void *buf, size_t nbytes);
```
+ `fildes`의 현재 포인터가 가리키고 있는 곳으로 부터 첫 `nbytes`를 읽어 `buf`에 저장한다
+ `return` 값은 읽어온 바이트 수이고, 만약 오류가 생긴 경우 `-1`을 리턴한다.

```c
#include <unistd.h>
#include <stdlib.h>

int main() {
	char buffer[128];
	int nread;

	nread = read(0, buffer, 128);
	if (nread == -1)
		write(2, "A read error has occurred\n", 26);
	exit(0);
```
##### open
```c
#include <fcntl.h>
#include <sys/types.h>
#include <sys/stat.h>

int open(const char *path, int oflags);
int open(const char *path, int oflags, mode_t mode);
mode_t umask(mode_t mode);
```
+ `path`에 해당되는 파일 혹은 디렉토리를 열어 읽어 온다. 이때 `oflags`나 `mode`를 통해 여러 모드 및 권한를 지원
+ `oflags`: `|`를 통해 함께 여러 `oflags`를 적용할 수 있음
	+ `O_RDONLY`: read-only
	+ `O_WRONLY`: write-only
	+ `O_RDWR`: read and write
	+ `O_APPEND`: 데이터를 파일의 끝부터 입력(기존 파일에 데이터를 추가)
	+ `O_TRUNC`: 기록되어 있는 파일의 데이터를 무시하고 새로 입력
	+ `O_CREAT`: 필요하다면 파일을 새로 생성. 권한은 `mode`로 결정
	+ `O_EXCL`: `O_CREAT`와 함께 사용되는 경우 파일이 없는 경우 생성하고, 있으면 오류를 출력
+ `mode`: `O_CREAT`와 함께 사용되는 경우 사용하는 변수로, `|`를 통해 함께 여러 권한을 설정할 수 있음
	+ `S_IRUSR`, `S_IWUSR`, `S_IXUSR`: owner의 읽기, 쓰기, 실행 권한
	+ `S_IRGRP`, `S_IWGRP`, `S_IXGRP`: group의 읽기, 쓰기, 실행 권한
	+ `S_IROTH`, `S_IWOTH`, `S_IXOTH`: others의 읽기, 쓰기, 실행 권한
+ `umask`: 파일이 생성될 때 기본 권한을 적용하는 것으로, `mode`와의 XOR 연산으로 파일의 권한이 결정 8진수의 3자리로 값이 결정된다. 
##### close
```c
#include <unist.h>

int close(int fildes);
```
+ `fildes`로 입력받은 파일이나 디렉토리를 닫는다. 
+ `return` 값은 실패한 경우 `-1` 성공한 경우 `1`
##### ioctl
```c
#include <unistd.h>

int ioctl(int fildes, int cmd, ...);
```
+ 하드웨어의 제어와 상태 정보를얻기 위해 사용.
+ `fildes`에게 `cmd`를 전달 해 명령을 전달

##### lseek
```c
#include <unistd.h>
#include <sys/types.h>

off_t lseek(int fildes, off_t offset, int whence);
```
+ `fildes`의 read/write pointer를 `offset`으로 읽어 오는 함수 `whence`로 설정을 바꿀 수 있음
+ `whence`
	+ `SEEK_SET`: 절대위치를 기준으로 `offset` 설정
	+ `SEEK_CUR`: 현재 위치에서 상대적인 위치를 기준으로 `offset` 설정
	+ `SEEK_END`: 맨 끝에서 상대적인 위치를 기준으로 `offset` 설정
+ 만약 `return` 값이 `-1`인 경우 pointer를 불러오는 데 실패

##### fstat, stat, lstat
```c
#include <unistd.h>
#include <sys/stat.h>
#include <sys/types.h>

int fstat(int fildes, struct set *buf);
int stat(const char *path, struct stat *buf);
int lstat(const char *path, struct stat *buf);
```
+ `fstat`은 `fildes`로부터 status information을 가져오는 함수
+ `stat`, `lstat`은 `path`를 읽어 status information을 가져오는 함수
+ Systemcall `stat <filename>`과 같은 동작을 수행한다

##### dup, dup2
```c
#include <unistd.h>

int dup(int fildes);
int dup2(int fildes, int fildes2);
```
+ `dup`은 `fildes`를 복제하여 반환하는데 가장 낮은 값으로 반환
+ `dup2`는 `fildes`를 `fildes2`로 지정하는데, 만약 `fildes2`가 이미 열려있으면 이를 닫고 복
+ 두 함수 모두 실패 시 `-1`를 `return`
##### Example
```c
#include <unistd.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <stdilb.h>

int main() {
	char c, block[1024];
	int in, out, nread;

	in = open("file.in", O_RDONLY);
	out = open("file.out", O_WRONLY | O_CREAT, S_IRUSR | S_IWUSR);
	while(read(in, &c, 1) == 1)
		write(out, &c, 1);
		
	close(in);
	close(out);
	
	//==============================================================
	
	in = open("file.in", O_RDONLY);
	out = open("file.out", O_WRONLY | O_CREAT, S_IRUSR | S_IWUSR);
	while((nread = read(in, block, sizeof(block))) > 0)
		write(out, block, nread);

	close(in);
	close(out);

	//==============================================================

	exit(0);
}
```
###### Implement `wc <filename>`
```c
#include <stdlib.h>
#include <stdio.h>
#include <string.h>
#include <errno.h>
#include <unistd.h>
#include <math.h>
#include <unistd.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <stdlib.h>

int main(int argc, char **argv)
{
	char buffer[128];
	int count_line, count_word, count_byte, file_id, nread, i, prev;
	file_id = open("infile", O_RDONLY);
	
	count_line = 0;
	count_word = 0;
	count_byte = 0;
	nread = 128;
	prev = 1;
	while(nread == 128) {
		nread = read(file_id, buffer, 128);
		count_byte += nread;
		for (i=0; i<nread; i++) {
			if (buffer[i] == '\n')
				count_line++;

			if (prev && !(buffer[i] == ' ' || buffer[i] == '\n')) {
				prev = 0;
				count_word++;
			}
			else if(buffer[i] == ' ' || buffer[i] == '\n') 
				prev = 1;
			
		}
	}

	close(file_id);

	printf("  %d  %d  %d infile\n", count_line, count_word, count_byte);
    exit(EXIT_SUCCESS);
}
```
###### Implement `split -n <filename>`
```c
#include <stdlib.h>
#include <stdio.h>
#include <string.h>
#include <errno.h>
#include <unistd.h>
#include <math.h>
#include <unistd.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <stdlib.h>

int main(int argc, char **argv)
{
	char buffer[128], buff[9];
	int line, open_id, out_id, nread, i, cnt, file_cnt, prev;
	mode_t old;
	
	if (argc < 2) {
		printf("Execution Error! Need integer value...\n\n");
		exit(EXIT_FAILURE);
	}
	
	line = atoi(argv[1]);
	open_id = open("infile", O_RDONLY);

	file_cnt = 0;
	cnt = 0;
	nread = 128;
	old = umask(0);
	sprintf(buff, "x%c%c", 97+((file_cnt/26)%26), 97+(file_cnt%26));
	out_id = open(buff, O_CREAT | O_WRONLY | O_TRUNC, 0644);

	while (nread == 128) {
		nread = read(open_id, buffer, 128);
		prev = 0;
		for(i=0; i<nread; i++) {
			if (buffer[i] == '\n') {
				write(out_id, buffer + prev, i-prev+1);
				
				cnt++;
				prev = i + 1;
			}

			if (cnt == line) {
				cnt = 0;
				close(out_id);
				file_cnt++;
				sprintf(buff, "x%c%c", 97+((file_cnt/26)%26), 97+(file_cnt%26));
				out_id = open(buff, O_CREAT | O_WRONLY | O_TRUNC, 0644);
			}
		}
		
		write(out_id, buffer + prev, nread-prev);

	}

	
	close(open_id);
	umask(old);
	close(out_id);
	
	printf("Execution finished successfully.... \n\n");
    exit(EXIT_SUCCESS);
}
```

#### High-level File Access
+ Standard I/O and error are descripted by number `stdin, stdout, stderr`
+ Others are descripted by stream `FILE *`

##### fopen
```c
#include <stdio.h>

FILE *fopen(const char *filename, const char *mode);
```
+ `filename`을 `mode`로 연다. 가능한 `mode`의 종류는 다음과 같다.
	+ `"r"` or `"rb"`: read only
	+ `"w"` or `"wb"`: write only, truncate to zero length
	+ `"a"` or `"ab"`: write only, append to end of file
	+ `"r+"` or `"rb+"` or `"r+b"`: read and write (for update)
	+ `"w+"` or `"wb+"` or `"w+b"`: read and write (for update), truncate to zero length
	+ `"a+"` or `"ab+"` or `"a+b"`: read and write (for update), append to end of file
+ `stdio.h`에 정의된 `FOPEN_MAX` 상수가 한번에 `open` 가능한 파일의 수를 제한 (8 or 16)
##### fread
```c
#include <stdio.h>

size_t fread(void *ptr, size_t size, size_t nitems, FILE *stream);
```
+ `stream`로부터 `nitems`개의 데이터를 읽어오는데, 각 데이터의 바이트 크기는 `size`만큼 읽어와 `ptr`에 저장
+ `return` 값은 읽어온 데이터의 수
##### fwrite
```c
#include <stdio.h>

size_t fwrite (const void *ptr, size_t size, size_t nitems, FILE *stream);
```
+ `stream`에 `ptr`에 저장된 `nitems`개의 `size` 바이트 크기의 데이터를 기록
+ `return` 값은 성공적으로 저장된 데이터의 수 
##### fclose
```c
#include <stdio.h>

int fclose(FILE *stream);
```
+ `stream`을 닫는다. high-level file access에서는 버퍼 데이터를 사용하기 때문에 매우 중요
##### fflush
```c
#include <stdio.h>

int fflush(FILE *stream);
```
+ 버퍼에 입력된 데이터를 실제로 `stream`에 기록
##### fseek
```c
#include <stdio.h>

int fseek(FILE *stream, long int offset, int whence);
```
+ `lseek`와 완전히 동일하게 작동하는 함수
##### fgetc, getc, getchar
```c
#include <stdio.h>

int fgetc(FILE *stream);
int getc(FILE *stream);
int getchar();
```
+ `fgetc`, `getc`는 `stream`의 다음 바이트를 `return`
+ `getchar`은 `getc(stdin)`과 동일
+ 만약 `return`값이 `EOF`인 경우 오류가 발생했거나 파일의 끝을 만난 경우이다. 이 경우 `ferror()`와 `feof()`로 구별 가능하다
##### fputc, putc, putchar
```c
#include <stdio.h>

int fputc(int c, FILE *stream);
int putc(int c, FILE *stream);
int putchar(int c);
```
+ `fputc`, `putc`는 문자 하나를 `stream`에 입력하는 함수 
+ `putchar(c)`은 `putc(c, stdout)`과 동일
+ 만약 입력에 실패하는 경우 `EOF`를 `return`
##### fgets, gets
```c
#include <stdio.h>

char *fgets(char *s, int n, FILE *stream);
char *gets(char *s);
```
+ `fgets`는 `stream`으로 최대 길이가 `n-1`인 string을 읽어와 `s`에 저장하는 함수
+ `\n`을 만날 때 까지 문자를 읽어 저장하고, 최대 `n-1`만큼 읽는다. 
+ `gets(s)`는 `fgets(s, sizeof(s), stdin)`과 동일하다
+ 정상적으로 수행된 경우 `s`를 `return`하고 오류가 발생하거나 읽을 문자가 없는 경우 `NULL`를 `return`

##### Formatted Input and Output
###### printf, fprintf, sprintf
```c
#include <stdio.h>

int printf(const char *format, ...);
int sprintf(char *s, const char *format, ...);
int fprintf(FILE *stream, const char *format, ...);
```
+ 서식 문자`%?`를 이용해 문장을 형식화해 `stdout`, `s`, `stream`에 입력하는 함수

###### scanf, fscanf, sscanf
```c
#include <stdio.h>

int scanf(const char *format, ...);
int sscanf(char *s, const char *format, ...);
int sscanf(FILE *stream, const char *format, ...);
```
+ 서식 문자 `%?`를 이용해 `stdin`, `s`, `stream`에서 원하는 데이터를 각각 필요한 형태로 읽어오는 함수
##### Other Stream Functions
+ `fgetpos`: file stream에서 pointer의 현재 위치를 가져옴
+ `fsetpos`: file stream에서 pointer의 위치를 설정
+ `ftell`: 현재 파일의 offset를 반환
+ `rewind`: file stream에서 pointer의 위치를 리셋
+ `freopen`: file stream을 다시 오픈
+ `setvbuf`: stream을 위한 buffering scheme을 설정
+ `remove`: `unlink`와 동일한 명령으로, directory가 아닌 `path`에 대한 명령 수행. `rmdir`과 다르다.

##### Stream Errors
```c
#include <errno.h>
#include <string.h>
#include <stdio.h>

extern int errno;
char *strerror(int errnum);
int ferror(FILE *stream);
int feof(FILE *stream);
void clearerr(FILE *stream);
```
##### Streams and File Descriptors
```c
#include <stdio.h>

int fileno(FILE *stream);
FILE *fdopen(int fildes, const char *mode);
```
+ low-level과 high-level을 함께 사용할 수 있도록 지원하기 위해 존재하는 함수. 
+ 하지만 자주 사용하지는 않는 형태. (하나의 형태로 유지해 사용)
##### Example
```c
#include <stdio.h>
#include <stdlib.h>

int main() {
	int c;
	FILE *in, *out;

	in = fopen("file.in", "r");
	out = fopen("file.out", "w");

	while ((c = fgetc(in)) != EOF)
		fputc(c, out);

	exit(0);
}
```
###### Implement `split -n`
```c 
#include <stdlib.h>
#include <stdio.h>
#include <string.h>
#include <errno.h>

#define LINE_MAX	1024

int main(int argc, char **argv)
{
	if (argc < 2) {
		printf("Execution Error! Need integer value...\n\n");
		exit(EXIT_FAILURE);
	}

	char out_file[9], buffer;
	int file_cnt, line, cnt;
	FILE *in, *out;
	
	line = atoi(argv[1]);

	in = fopen("infile", "r");
	file_cnt = 0;
	cnt = 0;

	sprintf(out_file, "x%c%c", 97+((file_cnt/26)%26), 97+(file_cnt%26));
	out = fopen(out_file, "w");

	while ((buffer = fgetc(in)) != EOF) {
		if(buffer == '\n') 
			cnt++;
		fputc(buffer, out);

		if (cnt == line) {
			cnt = 0;
			file_cnt++;
			fclose(out);
			sprintf(out_file, "x%c%c", 97+((file_cnt/26)%26), 97+(file_cnt%26));
			out = fopen(out_file, "w");
		}
	}
	fclose(in);
	fclose(out);

	printf("Execution finished successfully.... \n\n");
    exit(EXIT_SUCCESS);
}
```
###### Changes All Lower-case to Upper-case in the File
```c
#include <stdlib.h>
#include <stdio.h>
#include <string.h>
#include <errno.h>
#include <unistd.h>
#include <math.h>
#include <unistd.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <stdlib.h>

int main(int argc, char **argv)
{
	char buffer;
	FILE *file_id;
	file_id = fopen("infile", "r+");
	
	while ((buffer = fgetc(file_id)) != EOF) {
		if(buffer >= 97 && buffer <= 122) 
			buffer -= 32;
		fseek(file_id, -1, SEEK_CUR);
		fputc(buffer, file_id);
	}
	

	fclose(file_id);

	printf("Execution finished successfully.... \n\n");
	exit(EXIT_SUCCESS);
}
```
#### File and Directory Maintenance
##### chmod
```c
#include <sys/stat.h>

int chmod(const char *path, mode_t mode);
```
+ 파일이나 디렉토리의 권한을 변경하는 함수
##### chown
```c
#include <sys/types.h>
#include <unistd.h>

int chown(const char *path, uid_t owner, gid_t group);
```
+ 관리자 유저가 사용할 수 있는 함수로, 파일의 owner를 바꾸는 함수

##### unlink, link, symlink
```c
#include <unistd.h>

int unlink(const char *path);
int link(const char *path1, const char *path2);
int symlink(const char *path1, const char *path2);
```
+ 디렉토리에 존재하는 파일의 링크를 제거하거나 디렉토리에 링크를 생성하는 함수
+ `rm`, `ln`이 이 함수를 이용해 링크를 제거하거나 링크를 생성
###### Exmple
```shell
$ touch a
$ ls -il
total 0
4194520 -rw-rw-r-- 1 s2019270664 s2019270664 0 Oct 21 16:45 a
$ ln a b
$ ls -il
total 0
4194520 -rw-rw-r-- 2 s2019270664 s2019270664 0 Oct 21 16:45 a
4194520 -rw-rw-r-- 2 s2019270664 s2019270664 0 Oct 21 16:45 b
$ ln b c
$ ls -il
total 0
4194520 -rw-rw-r-- 3 s2019270664 s2019270664 0 Oct 21 16:45 a
4194520 -rw-rw-r-- 3 s2019270664 s2019270664 0 Oct 21 16:45 b
4194520 -rw-rw-r-- 3 s2019270664 s2019270664 0 Oct 21 16:45 c
$ unlink a
$ ls -il
total 0
4194520 -rw-rw-r-- 2 s2019270664 s2019270664 0 Oct 21 16:45 b
4194520 -rw-rw-r-- 2 s2019270664 s2019270664 0 Oct 21 16:45 c
```

```shell
$ touch a
$ ln a b
$ ln -s a c
$ ln -s b d
$ ls -il
total 0
4194520 -rw-rw-r-- 2 s2019270664 s2019270664 0 Oct 21 16:47 a
4194520 -rw-rw-r-- 2 s2019270664 s2019270664 0 Oct 21 16:47 b
4194521 lrwxrwxrwx 1 s2019270664 s2019270664 1 Oct 21 16:47 c -> a
4194522 lrwxrwxrwx 1 s2019270664 s2019270664 1 Oct 21 16:47 d -> b
$ echo hello > a
$ cat b
hello
$ echo Kim >> d
$ cat a
hello
Kim
```
##### mkdir, rmdir
```c
#include <sys/types.h>
#include <sys/stat.h>
#include <unistd.h>

int mkdir(const char *path, mode_t mode);
int rmdir(const char *path);
```
+ `mkdir`은 `mode`로 `path`인 디렉토리 생성
+ `rmdir`은 `path`인 디렉토리 삭제. 이때 비어있는 디렉토리에 대해서만 삭제 가능
##### chdir, getcwd
```c
#include <unistd.h>

int chdir(const char *path);
char *getcwd(char *buf, size_t size);
```
+ `chdir`은 `path`로 현재 디렉토리 변경
+ `getcwd`는 현재 디렉토리의 경로를 `buf`의 첫 `size` 바이트 만큼 저장
##### Example
###### Make Directories
```c
#include <stdlib.h>
#include <stdio.h>
#include <string.h>
#include <errno.h>
#include <unistd.h>
#include <math.h>
#include <unistd.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <stdlib.h>

#define MAX_NAME_LENGTH 26

int main(int argc, char **argv)
{
	int i;
	char dir_name[MAX_NAME_LENGTH] = "a";

	for(i=0; i<14; i++) {
		dir_name[0] = 'a'+ i;
		mkdir(dir_name, 0777);
		chdir(dir_name);
	}

	printf("Execution finished successfully.... \n\n");
    exit(EXIT_SUCCESS);
}
```
#### Scanning Directories
+ use `DIR` structure 
##### opendir
```c
#include <sys/types.h>
#include <dirent.h>

DIR *opendir(const char *name);
```
+ `name`인 디렉토리를 오픈. 실패 시 `NULL`를 `return`
##### readdir
```c
#include <sys/types.h>
#include <dirent.h>

struct dirent *readdir(DIR *dirp);
```
+ `dirp`로부터 현재 pointer가 위치한 `struct dirent`를 읽어온다. 실패 시 `NULL`를 `return`
##### telldir
```c
#include <sys/types.h>
#include <dirent.h>

long int telldir(DIR *dirp);
```
+ `dirp`의 현재 위치를 리턴한다.
##### seekdir
```c
#include <sys/types.h>
#include <dirent.h>

void seekdir(DIR *dirp, long int loc);
```
+ `dirp`의 위치 pointer를 읽어온다. 
##### closedir
```c
#include <sys/types.h>
#include <dirent.h>

int closedir(DIR *dirp);
```
+ `dirp`를 닫는다. 실패 시 `-1`,성공 시 `1`를 `return`

##### Example
###### Directory-Scanning Program
```c
#include <unistd.h>
#include <stdio.h>
#include <dirent.h>
#include <string.h>
#include <sys/stat.h>
#include <stdlib.h>

void printdir(char *dir, int depth) {
	DIR *dp;
	struct dirent *entry;
	struct stat statbuf;

	if ((dp = open(dir)) == NULL) {
		fprintf(stdeer, "cannot open directory: %s\n", dir);
	}

	chdir(dir);
	while ((entry = readdir(dp)) != NULL) {
		lstat(entry->d_name, &statbuf);
		if (S_ISDIR(statbuf.st_mode)) {
			if (strcmp(".", entry->d_name) == 0 | strcmp("..", entry->d_name) == 0)
				continue;

			printf("%*s%s/\n", depth, "", entry->d_name);
			printdir(entry->d_name, depth+4);
		}
		else
			printf("%*s%s/\n", depth, "", entry->d_name);
	}
	chdir("..");
	closedir(dp);
}

int main(int argc, char *argv[])) {
	printf("Directory scan of /home:\n");
	printdir("/home", 0);
	printf("done.\n");

	// =========================================
	
	char *topdir = ".";
	if (argc >= 2)
		topdir = argv[1];

	printf("Directory scan of %s:\n", topdir);
	printdir(topdir, 0);
	printf("done.\n");
	
	eixt(0);
}
```

###### Print max depth
```c
#include <stdlib.h>
#include <stdio.h>
#include <string.h>
#include <errno.h>
#include <unistd.h>
#include <math.h>
#include <unistd.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <stdlib.h>
#include <dirent.h>

int g_max_depth=0;

void travelDir(char *dir, int depth) {
	DIR *dp;
	struct dirent *cnt;
	struct stat statbuf;

	// error checking
	if((dp = opendir(dir)) == NULL) {
		fprintf(stderr, "cannot open directory: %s\n", dir);
		return;
	}

	chdir(dir);
	while ((cnt = readdir(dp)) != NULL)
	{
		lstat(cnt->d_name, &statbuf);	// read file status

		// check if the file is a directory
		if(S_ISDIR(statbuf.st_mode)) {
			
			// ignore "." and ".."
			if(strcmp(".", cnt->d_name) == 0 || strcmp("..", cnt->d_name) == 0)
				continue;

			travelDir(cnt->d_name, depth+1);
		}
	}

	chdir("..");
	closedir(dp);

	if(g_max_depth < depth) 
		g_max_depth = depth;
}

int main(int argc, char **argv)
{
	if( argc < 2 ) {
		printf("Usage: ] ./ret <dir path>\n");
		exit(1);
	}

	int i;
	printf("%d\n", argc);
	for(i=0; i<argc; i++) 	printf("%s\n", argv[i]);
	

	for(i=1; i<argc; i++)
		travelDir(argv[i], 0);	

    printf("Max Depth = %d\n", g_max_depth);
	printf("Execution finished successfully.... \n\n");
    exit(EXIT_SUCCESS);
}
```

#### Errors
+ 에러에 대한 내용은 `errrno.h`에 모두 저장되어 있다. 몇 가지 예시는 다음과 같다.
+ `EPERM`: operation not permitted
+ `ENOENT`: no such file or directory 
+ `EINTR`: interrupted system call
+ `EIO`: I/O error
+ `EBUSY`: device or resource busy
+ `EEXIST`: file exists
+ `EINVAL`: invalid argument
+ `EMFILE`: too many open file
+ `ENODEV`: no such device
+ `EISDIR`: is a directory
+ `ENOTDIR`: isn't a dircetory
##### strerrer
```c
#include <string.h>

char *strerror(int errnum);
```
+ `errnum`을 문장으로 매핑해주는 함수
##### perror
```c
#include <stdio.h>

void perror(const char *s);
```
+ `errno`에 의해 발생한 현재 에러를 `s`에 저장하면서 `stderr`에 출력해주는 함수

#### /proc File System
Linux에서는 process도 모두 파일로서 관리를 수행한다. 그러므로 모든 프로세스에 대한 정보들을 `/porc` 디렉토리에 저장되게 된다. 이 디렉토리는 디스크에 저장되는 것이 아니라 시스템이 부팅될 때 생성되는 디렉토리이다. 
+ `/proc/cpuinfo`: details of the processors available
+ `/proc/meminfo`: information about memory usage
+ `/proc/version`: information about the kernel version
+ `/proc/net/sockstat`: network socket usage statistics

여러가지 설정 값을 `/proc`파일에서 수정해 프로세스에 대한 설정을 변경할 수 있다
```shell
$ cat /proc/sys/fs/file-max
76583
$ echo 80000 > /proc/sys/fs/file-max
$ cat /proc/sys/fs/file-max
80000
```

#### mmap
##### mmap
```c
#include <sys/mman.h>

void *mmap(void *addr, size_t len, int prot, int flags, int fildes, off_t off);
```
+ 메모리 영역의 포인터를 생성하는 함수로, `fildes`의 내용을 메모리에 써놓고 사용하기 위해 사용하는 함수
##### msync
```c
#include <sys/mman.h>

int msync(void *addr, size_t len, int flags);
```
+ 모든 memory segment나 일부분을 모두 처음으로 되돌리는 함수
##### Example
```c
#include <unistd.h>
#include <stdio.h>
#include <sys/mman.h>
#include <fcntl.h>
#include <stdlib.h>

typedef struct {
	int integer;
	char string[24];
} RECORD;

#define NRECORDS (100)

int main() {
	RECORD record, *mapped;
	int i, f;
	FILE *fp;

	fp = fopen("records.dat", "w+");
	for(i=0; i<NRECORDS; i++) {
		record.integer = 1;
		sprtinf(record.string, "RECORD-%d", i);
		fwrite(&record, sizeof(record), 1, fp);
	}
	fclose(fp);

	// ==============================================
	// use high-level file access

	fp = fopen("records.dat", "r+");
	fseek(fp, 43*sizeof(record), SEEK_SET);
	fread(&record, sizeof(record), 1, fp);

	record.integer = 143;
	sprintf(record.string, "RECORD-%d", record.integer);

	fseek(fp, 43*sizeof(record), SEEK_SET);
	fwrite(&record, sizeof(record), 1, fp);
	fclose(fp);
	
	// ==============================================
	// use mmap

	f = open("records.dat", O_RDWR);
	mapped = (RECORD *)mmap(0, NRECORDS*sizeof(record), RPOT_READ | RPOT_WRITE, MAP_SHARED, f, 0);
	mapped[43].integer = 243;
	sprintf(mapped[43].string, "RECORD-%d", mapped[43].integer);

	msync((void *)mapped, NRECORDS*sizeof(record), MS_ASYNC);
	munmap((void *)mapped, NRECORDS*sizeof(record));
	close(f);

	eixt(0);
}
```
1. change the integer value of `record 43` to `143` and write this to the `43rd record's string`
2. map the records into memory and access the `43rd record` in order to change the intger to `243` using memory mapping