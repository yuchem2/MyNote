#### Time and Date
시간과 날짜를 정의하는 3개의 데이터 형식이 존재하고, 이를 서로 치환하는 함수들이 존재한다. 
```c
#include <time.h>
struct tm {
	int tm_sec;  // 0~59
	int tm_min;  // 0~59
	int tm_hour; // 0~23
	int tm_mday; // 1~31
	int tm_mon;  // 0~11
	int tm_year; // 1900 + x
	int tm_wday; // 0~6
	int tm_yday; // 0~365
	int tm_isdst; // daylight svaing time
};
typedef time_t unsigned long; 
char* ___;
```
##### Functions
![[Pasted image 20231215135408.png | 600]]
```c
#include <time.h>
time_t time(time_t *tloc);
double difftime(time_t time1, time_t time2);

struct tm *gmtime(const time_t *timeval);
struct tm *localtime(const time_t *timeval);

time_t mktime(struct tm *timeptr);

char *astime(const struct tm *timeptr);
char *ctime(const time_t *timeval);
size_t strftime(char *s, size_t maxsize, const char *format, struct tm *timeptr);
size_t strptime(const char *buf, const char *format, struct tm *timeptr);
```
+ `time()`: `tloc`부터 현재까지 지나간 시간을 리턴 (`long` 값을 가짐)
+ `difftime()`: 두 시간의 차이를 리턴
+ `gmtime()`: GMT+0을 기준으로 `time_t`를 `stuct tm` 형태로 리턴
+ `localtime()`: 현재 지역시간을 기준으로 `time_t`를 `struct tm` 형태로 리턴
+ `mktime()`: `struct tm` 형태를 `time_t`로 변경
+ `astime()`, `ctime()`: `struct tm`, `time_t` 형태의 값을 정해진 `char*` 형태의 format으로 리턴
+ `strftime()`, `strptime()`: `struct tm` 형태의 값을 format을 지정해 문자열로 리턴/문자열에 존재하는 날짜 정보를 format을 통해 `time_tm`으로 리턴. 사용할 수 있는 형식 지정자는 다음과 같다.
	+ `%a`, `%A`: 요일 리턴
	+ `%b`, `%B`: 달 이름 리턴
	+ `%c`: 날짜와 시간
	+ `%d`: 달 기준 일 (01~31)
	+ `%H`: 시간
	+ `%I`: 12시간 기준 시간
	+ `%j`: 년도 기준 일 (001~366)
	+ `%m`: 년도 기준 달(01~12)
	+ `%M`: 분 (00~59)
	+ `%p`: 오후, 오전
	+ `%S`: 초 (00~61)
	+ `%u`, `%W`: 주 기준 일 (1~7/0~6)
	+ `%U`, `%V`: 년도 기준 주차 (일요일/월요일 시작)
	+ `%x`, `%X`: local format의 날짜/시간
	+ `%y`, `%Y`: 1900 + y 중 y 리턴/1900+y 리턴
	+ `%Z`: 시간 영역대 이름
	+ `%%`: % 글자

##### Example
###### strftime.c
```c
#define _XOPEN_SOURCE
#include <time.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main() {
	struct tm *tm_ptr, timestruct;
	time_t the_time;
	char buf[256];
	char *result;

	(void) time(&the_time);
	tm_ptr = localtime(&the_time);
	strftime(buf, 256, "%A %d %B, %I:%S %p", tm_ptr);
	printf("strftime gives: %s\n", buf);

	strcpy(buf, "The 26 July 2007, 17:53 will do fime");
	printf("calling strptime with %s\n", buf);
	tm_ptr = &timestruct;

	result = strptime(buf, "%a %d %b %Y %R", tm_ptr);
	printf("strptime consumed up to: %s\n", result);

	printf("strptime gives:\n");
	printf("date: %02d/%02d/%02d\n", tm_ptr->tm_year%100, tm_ptr->tm_mon+1, tm_ptr->tm_mday);
	printf("time: %02d:%02d\n", tm_ptr->tm_hour, tm_ptr->tm_min);
	exit(0);
}
```

###### Calendar
```c
#include <stdlib.h>
#include <stdio.h>
#include <string.h>
#include <errno.h>
#include <unistd.h>
#include <math.h>
#include <time.h>
#include <sys/types.h>

void print_calendar(int year, int month) {
	time_t base = 0;
	struct tm* timeval;
	int months[] = {31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};
	int i, buff, day;

	// check year is leap year
	if(((year % 4 == 0) && (year % 100 != 0)) || (year % 400 == 0))
		months[1] += 1;

	// calculate times from 1970(base)
	if (year >= 1970) {
		for (i=1970; i<year; i++) {
			if(((i % 4 == 0) && (i % 100 != 0)) || (i % 400 == 0))
				base += 366;
			else
				base += 365;
		}
		base = base*24*60*60;
	}
	else {
		for (i=year; i<1970; i++) {
			if(((i % 4 == 0) && (i % 100 != 0)) || (i % 400 == 0))
				base += 366;
			else
				base += 365;
		}
		base = -base*24*60*60;
	}
	
	buff = 0;
	for(i=0; i<month-1; i++)
		buff += months[i];
	base += buff*24*60*60;
	
	// find first day of month
	timeval = localtime(&base);

	// print calendar
	printf("\n            %2d %4d\n", month, year);
	printf("Sun  Mon  Tue  Wed  Thu  Fri  Sat\n");
	for(i=0; i<timeval->tm_wday; i++)
		printf("     ");

	day = 1;
	while(day<=months[month-1]) {
		printf("%3d  ", day);
		if(i==6)
			printf("\n");
		i = (i+1)%7;
		day += 1;
	}
	printf("\n");
}


int main(int argc, char **argv) {
	if (argc != 1) {
		printf("USAGE: ] ret\n");
		exit(0);
	}

	print_calendar(2023, 8);
	print_calendar(2023, 9);
	print_calendar(2023, 10);
	print_calendar(2023, 11);
	print_calendar(2023, 12);
}
```
#### User Information
##### UID
```c
#include <sys/tpyes.h>
#include <unistd.h>

uid_t getuid(void);
char *getlogin(void);
```
+ `getuid()`: program의 접속 UID를 리턴. 이 값은 일반적으로 program을 실행한 유저의 id가 된다. 
+ `getlogin()`: 현재 유저의 login name을 리턴
+ `/etc/passwd` 파일에 user 계정에 대한 정보가 저장되어 있다.
##### Password
```c
#include <sys/types.h>
#include <pwd.h>

struct passwd {
	char *pw_name;    // user name
	char *pw_passwd;  // user password
	uid_t pw_uid;     // user ID
	gid_t pw_gid;     // group ID
	char *pw_gecos;   // real name
	char *pw_dir;     // home directory
	char *pw_shell;   // shell program
};

struct passwd *getpwuid(uid_t uid);
struct passwd *getpwnam(const char *name);
struct passwd *getpwent(void);
void endpwent(void);
void setpwent(void);
```
+ `getpwuid()`, `getpwnam`: UID, 유저 이름을 받아 해당하는 유저의 비밀번호 정보를 리턴
+ `getpwent`: 현재 유저 정보 entry가 가리키고 있는 유저 정보를 리턴
+ `endpwent`: 유저 정보 스캔을 멈춤
+ `setpwent`: `password` 파일의 처음으로 돌아가 다시 시작
##### User and Group identifiers
```c
#include <sys/types.h>
#include <unistd.h>

uid_t geteuid(void);
gid_t getgid(void);
gid_t getegid(void);
int setuid(uid_t uid);
int setgid(gid_t gid);
```
+ 현재 프로그램의 effective UID, GID, effective 를 획득할 수 있음. 이를 이용해 현재 프로그램의 실제 UID, GID가 아닌 다른 값으로 변경해 권한을 바꿀 수 있음.
#### Host Information
##### Hostname
```c
#include <unistd.h>

int gethostname(char *name, size_t namelen);
```
+ `gethostname()`: 장치의 network name을 리턴
##### Uname
```c
#include <sys/utsname.h>

struct utsname {
	char sysname[];
	char nodename[];
	char release[];
	char vesion[];
	char machine[];
#ifdef _GNU_SOURCE
	char domainname[];
#endif
};

int uname(struct utsname *uname);
```
+ `uname()`: host computer의 상세 정보 리턴

