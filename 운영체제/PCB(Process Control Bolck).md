
[[운영체제(Operating System)]]의 heap 영역에 저장되어 있으며 각 [[Process]]는 OS에 의해 이 곳에 위치해 있다

각 프로세스와 연관된 정보를 가지고 있다
+ Process state
+ [[PC(Program Counter)]]
+ CPU [[레지스터(Register)]]: Accumlators, index register, stack pointer, general-purpose register
+ CPU Scheduling information: 프로세스 우선순위, pointers to scheduling queue
+ Memory-management information:  value of base and limit registers, the page tables or 
segment tables, etc.
+ Accounting information:  amount of CPU and real time used, time limits, account number, 
job and process number
+ I/O status information: I/O status information: list of I/O devices allocated, open files, etc. 
