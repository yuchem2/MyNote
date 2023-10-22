### Complie, Link, and Run 
1. Object file 없이 실행 파일 생성 후 실행
```shell
$ gcc -o hello hello.c
$ ./hello
```

2. Use subdirectories or nonstandard *header files*
```shell
$ gcc -I [dir path] fred.c
```

Default *header files* located: **/usr/include**
Addtional *header files* located: **/usr/include/X11** for X Window System,  **/usr/include/c++** for GNU C++

3. Use non standard llibraries
```shell
$ gcc -o fred fred.c -lm # search a library either by giving full path name or by using the -l
$ gcc -o xllfred -L [dir path] xllfred.c -lX11 # search directories 
```

Standard system *libraries* are usually stored in /`lib64` and `/usr/lib64`
A *library* filename always starts with lib. e.g. libc-2.17.so, libm-2.17.so
`.a` for traditional, *static libraries*, `.so` for *shared libraries*
`.so` libraries correspond to `.DLL` files and are required at run time.
`.a` libraries are similar to `.LIB` files included in the program executable

|      종류       | 프로그램에 적재 시기 | 사용 형태                                                                     |
|:---------------:|:--------------------:|:----------------------------------------------------------------------------- |
| Static Library  |     컴파일 타임      | 프로그램 내부에 적재되어 해당 프로그램만 사용가능                             |
| Dynamic Linking |   런타임(시작 시)    | 공유 메모리에 적재되어 해당 라이브러리를 사용하는 여러 프로그램에서 공유 가능 |
| Dynamic Loading | 런타임(실제 사용 시) | 공유 메모리에 적재되어 해당 라이브러리를 사용하는 여러 프로그램에서 공유가능  |

4. Make static Library
   + Create separated source files for each function
   + compile these functions individually to produce object files 
    ```shell
	$ gcc -c biil.c fred.c
	$ ls *.o
	bill.o    fred.o
	```
   + Write a program that calls the function	```
   + Create and use a library and Make a table of contents for the library
   ```shell
	$ ar crv libfoo.a biil.o fred.o
	a - bill.o
	a - fred.o
	$ ranlib libfoo.a
	```
   + Complie and test with the library
	```shell
	$ gcc -c program.c
	$ gcc -o program program.o ./libfoo.a    # select one
	$ gcc -o program program.o -L./ -lfoo    # select one
	$ ldd ./ program
	$ ./program
	bill: we passed Hello World
	```
   + Standard Method
	```shell
	$ gcc -c fred.c biil.c
	$ ar crv libfoo.a fred.o bill.o
	$ ranlib libfoo.a
	$ mv ./ilbfoo.a /lib64/
	$ gcc -c program.c
	$ gcc -o program program.o -lfoo
	$ ldd ./program
	$ ./program
	```

5. Make Shared Library
```shell
$ gcc -fPIC -c bill.c fred.c
$ gcc -shared -Wl, -soname, libfoo.so -o libfoo.so.1.0.1 bill.o fred.o
$ mv libfoo.so.1.0.1 /usr/lib64/
$ idconfig
$ ln -s /usr/lib64/libfoo.so.1.0.1 /usr/lib64/libfoo.so #create link file
$ gcc -c program.c
$ gcc -o program program.o -lfoo
$ ldd program
$ ./program
```

6. Getting Help: use man command
```shell
$ yum install man
$ man gcc
$ man -a printf
$ man 3 abs
```
