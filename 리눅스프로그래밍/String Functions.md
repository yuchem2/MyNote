### Function of string
```c
#include <string.h>

size_t strlen(const char *s);                 // calculate the length of string

int strcmp(const char *s1, const char *s2);   // compare two string, return negative, zero, positive
int strncmp(const char *s1, const char *s2, size_t n)    // compare only first n bytes

char *strcpy(char *dest, const char *src);               // copies at the string pointeg to by src
char *strncpy(char *dest, const char *src, size_t n);    // copies at most n bytes of src

char *strcat(char *dest, const char *src);               // appends the src string to the dest 
char *strncat(char *dest, const char *src, size_t n);    // appends at most n bytes of src

char *strchr(const char *s, int c);      // returns a pointer to the first occurrence of c
char *strrchr(const char *s, int c);     // returns a pointer to the last occurrence of c
char *strstr(const char *haystack, const char *needle); // return the first occurrence of needle

char *strtok(char *str, const char *delim);
// breaks a string into a sequence of zero or more nonempty tokens
```

```c
#include <stdilb.h>

int atoi(const char *nptr);           // string to int
long atol(const char *nptr);          // string to long int
long long atoll(const char *nptr);    // string to long long int
double atof(const char *nptr);        // string to double

long int strtol(const char *nptr, char **endptr, int base); 
// converts the inital part of the string in nptr
// to a long integer value according to the given base (0, 2-36)

long long int strtoll(const char *nptr, char **endptr, int base);
// converts the inital part of the string in nptr
```

##### Example
###### Inplment `grep -n -i <pattern> <filename>`
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
#include <ctype.h>

#define MAX_LINE	512
#define MAX_STR		128

char *strlwr(char *dest, char *src) {
	int srcLine = strlen(src);

	int i;
	for(i=0; i<srcLine; i++) {
		if(src[i] >= 'A' && src[i] <= 'Z')
			dest[i] = src[i]+32;
		else
			dest[i] = src[i];
	}
	dest[srcLine] = '\0';

	return dest;
}

char *strupr(char *dest, char *src) {
	int srcLine = strlen(src);

	int i;
	for(i=0; i<srcLine; i++) {
		if(src[i] >= 'a' && src[i] <= 'z')
			dest[i] = src[i]-32;
		else
			dest[i] = src[i];
	}
	dest[srcLine] = '\0';

	return dest;
}

int main(int argc, char **argv)
{

	if (argc != 3) {
		printf("Usage: ]$ ./ret <pattern> <file>\n");
		exit(1);
	}

	char pattern1[MAX_STR],pattern2[MAX_STR];
	strlwr(pattern1, argv[1]);
	strupr(pattern2, argv[1]);

	char *file = argv[2];
	char buffer[MAX_STR] = "";
	FILE *in;
	int line = 1;
	

	in = fopen(file, "r");
	while (fgets(buffer, MAX_STR, in) != NULL && line < MAX_LINE) {
		if (strstr(buffer, pattern1) || strstr(buffer, pattern2))	
			printf("\033[0;32m%d\033[0m:%s", line, buffer);
		line++;
	}

	fclose(in);
    exit(EXIT_SUCCESS);
}
```
###### Implement `sort <filename>`
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

#define MAX_RECORD	100
#define MAX_LINE	512

int main(int argc, char **argv)
{

	if( argc != 2 ) {
		printf("Usage: ]$ ./ret <file name>\n");
		exit(1);
	}

	char record[MAX_RECORD][MAX_LINE], temp[MAX_LINE];
	FILE *in;
	int i, j, line=0;

	in = fopen(argv[1], "r");
	while (fgets(record[line], MAX_LINE, in) != NULL && line < MAX_RECORD) {
		line++;
	}
	
	for(i=line; i>0; i--) {
		for(j=0; j<i; j++) {
			if (strcmp(record[j], record[j+1]) > 0) {
				strcpy(temp, record[j]);
				strcpy(record[j], record[j+1]);
				strcpy(record[j+1], temp);
			}
		}
	}

	for(i=0; i<=line; i++) {
		printf("%s", record[i]);
	}

	fclose(in);
    exit(EXIT_SUCCESS);
}
```