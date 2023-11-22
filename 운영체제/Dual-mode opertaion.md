
User mode와 Kernel mode가 존재한다

Mode bit에 의해 mode가 변경되며 이는 HW에 의해 제공된다.

다음과 같은 기본적인 기능들이 존재한다 
+ 시스템이 user code와 kernel code가 실행될 때를 구별할 수 있도록 함
+ 몇몇 명령어들은 kernel mode에서만 실행될 수 있도록 특권을 제공
+ System call은 kernel mode로 전환하였다가 호출로부터 리셋되어 다시 user mode로 복귀한다


### 전환 예시
1. 시스템 부팅시 HW는 kernel mode로 실행한다
2. 그 후 [[운영체제(Operating System)]]가 적재되고, user mode의 사용자 응용 프로그램이 실행된다
3. [[트랩(Trap)]]이나 [[인터럽트(Interrupt)]]가 발생할 때마다 HW는 kernel mode로 전환한다(mode bit=0)
4. 사용자 프로그램에게 제어권을 넘겨 주기 전에 user mode로 전환한다

