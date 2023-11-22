## Background
+ 공유 데이터에 대한 동시 접근은 데이터 일관성을 깨뜨리는 결과를 초래함
+ 데이터 일관성을 유지하기 위해서는 [[IPC (InterProcess Communication)]]를 수행하는 프로세스들이 순차적으로 수행되는 것을 보장하는 방법이 요구됨

e.g. 모든 버퍼를 다 채우는 [[Producer-Consumer Problem]]의 해결책은 다음과 같다. 
+ 버퍼에 있는 아이템의 개수를 기록하는 counter라는 정수를 통해 해결
+ 처음에 counter는 0으로 초기화
+ Producer가 아이템을 생성한 후 producer에 의해 값이 증가
+ Consumer가 아이템을 소비한 후 consumer에 의해 값이 감소

```c
#define BUFFER_SIZE 10

typedef struct{
	int content;
} item;

item buffer[BUFFER_SIZE]
int in = 0;
int out = 0;
int counter = 0;

// producer
void insert(void) {
	item nextProduced;
	while (TRUE) {
		// Produce an item in nextProduced
		while (counter == BUFFER_SIZE) ;
		// do nothing
	
		// insert an item into the buffer
		buffer[in] = nextProduced;
		in = (in + 1) % BUFFER_SIZE;
	
		counter++;
	}
}

// consumer
item remove(void) {
	item nextConsumed;

	while(count == 0) ;
	// do nothing

	// remove an item from the buffer
	nextConsumed = buffer[out];
	out = (out + 1) % BUFFER_SIZE;
	counter--;
	
	return nextConsumed;
}
```
## [[Race Condition]]
![[Race Condition]]
## [[CS(Critical-Section) Problem]]
![[CS(Critical-Section) Problem]]
### Solution
CS 문제를 해결하기 위해서는 다음과 같은 조건을 만족해야 한다. 
1. 상호 배제(Mutual Exclusion): 만약 프로세스 $P_i$가 CS를 수행하고 있다면, 다른 프로세스는 CS 수행 불가
2. 진행(Progress): 어떤 프로세스도 CS를 실행하지 않고 있을 때 CS에 들어가기를 희망하는 프로세스가 존재하면, 다음으로 CS에 들어갈 프로세스를 선택하는 것이 무한대로 지연되어서는 안된다. ([[Deadlock]]-free condition)
3. 유한 대기(Bounded Waiting):프로세스가 CS 진입 요청을 하고, 요청이 허락될 때까지 기다리는 시간은 유한해야 한다. ([[Starvation]]-free condition)
![[Deadlock]]

![[Starvation]]
#### Software-based
##### Peterson's Solution
두 프로세스 $\{P_i, P_j\}$간의 CS 문제를 해결하는 방법. LOAD, STORE 명령어가 ATOMIC하다고 가정할 때 두 프로세스는 다음 변수를 공유한다. 
```c
int turn;
boolean flag[2];
```
+ 변수 `turn`은 CS에 들어갈 차례를 나타낸다.
+ `flag` 배열은 프로세스가 CS에 들어갈 준비가 되었는지를 나타내는데 사용된다. 즉, `flag[i]=true`면, $P_i$는 CS에 들어갈 준비가 되었다는 의미를 내포한다.

각 process $P_i$는 다음과 같은 형태로 CS에 접근한다. 
```c
do {
	flag[i] = TRUE;
	turn = j;
	while(flag[j] && turn == j) ; // waiting 
	/*
		Critical Section
	*/
	flag[i] = FALSE;
	/*
		Remainder Section
	*/
} while(TRUE);
```
###### Satisfies 3 conditions
+ Mutual Exclusion: 각 $P_i$는 `flag[j] == falese`거나 `turn == i`인 경우에 CS에 진입. 각 $P_i$는 `flag[i]==true`인 상태에서 CS에 진입. $P_i, P_j$는 동시에 CS 접근 불가
+ Progress: $P_i$는 `flag[j] == true`이고, `turn == j`인 경우에만 멈춤
+ Bounded Waiting: $P_i$는 최대 $P_j$가 한번 CS에 진입한 후에 CS에 들어가게 된다.

만약 `while` 구문에서 둘 중 하나만 사용한다면 progress를 만족하지 못한다. 
##### Bakery Algorithm
n개의 프로세스가 존재할 때 해결법
```c
do {
	choosing[i] = TRUE; // 번호표 뽑기 시작
	number[i] = max(number[0], number[1], ..., number[n-1]) + 1; // watiing 중 최대 값 + 1
	choosing[i] = FALSE; // 번호표 뽑음. CS 들어가기 희망
	for(j=0; j<n; j++) {
		while(choosing[j]) ; // 누군가 번호표를 뽑으려고 하면 wait
		while((number[j] != 0) && (number[j], j) < (nubmer[i], i)) ; 
		// (a, b) < (c, d) if a < c or a == c and b < d
		// 다른 누군가가 기다리는 경우 자신의 값이 제일 작고, 순서가 앞설 때까지 wait
	}
	/*
		Critical Section
	*/
	number[i] = 0; // CS 사용 해제 알림
	/*
		Remainder Section
	*/
} while(TRUE);
```
##### Dekker's Algorithm
두 개의 프로세스가 존재할 때 해결법
```c
do {
	flag[i] = TRUE; // CS 들어가기 희망
	while(flag[j]) {
		// turn이 j이면, 번호표를 내려놓고, turn이 i가 될 때까지 wait
		if(turn == j) { 
			flag[i] = FALSE;
			while (turn == j) ;
			flag[i] = TRUE;
		}
	}
	/*
		Critical Section
	*/
	// CS 사용해제 알림
	turn = j;
	flag[i] = FALSE;
} while(TRUE);
```

#### Hardware based
많은 시스템이 CS를 위한 HW 지원을 제공한다. 
+ Uni-processors: [[인터럽트(Interrupt)]] 비활성화 가능. 
	+ 현재 수행되는 코드를 preemption없이 수행할 수 있음
	+ [[Multi-processor system]]에서는 비효율적. 이 방법을 사용하는 [[운영체제(Operating System)]]는 대체로 확장성이 없음
+ Modern machines: 특별한 ATOMIC 하드웨어 명령어 제공
	+ TestAndSet(): 메모리 워드 테스트와 값 설정
	+ Swap(): 두 메모리 워드 값을 교환
##### TestAndSet()
```c
boolean TestAndSet(boolean *target) {
	boolean rv = *target;
	*target = TRUE;
	return rv; // 입력된 파라미터를 true로 변경한다. 이때 return 값은 변경 전 파라미터 값
}
```
###### TestAndSet()을 이용한 해결법
+ Mutual Exclusion 해결가능
공유 변수를 `boolean lock`으로 설정하고, `false`로 초기화해 사용. 
```c
do {
	while(TestAndSet(&lock)) ; // 함수 호출 시점에서 lock이 false일 때 까지 wait
	/*
		Critical Section
	*/
	lock = FALSE;
	/*
		Remainder Section
	*/
} while(TRUE);
```
+ Bounded Waiting 해결 가능
공유 변수를 `boolean waiting[n]`과 `false`로 초기화된 `boolean lock`을 사용. 각 프로세스는 지역 변수 `boolean key`를 가짐
```c
do {
	waiting[i] = TRUE; // waiting start
	key = TRUE;
	while(waiting[i] && key) // 함수 호출 시점에서 lock이 false가 되거나 자신의 차례가 될때까지 
		key = TestAndSet(&lock); // busy waiting
	waiting[i] = FALSE; // waiting end
	/*
		Critical Section
	*/
	j = (i + 1) % n; // 내 다음 프로세스 번호
	while((j != i) && !waiting[j]) // waiting하고 있는 다음 프로세스를 찾음
		j = (j + 1) % n;
	if (j == i) // waiting하는 프로세스가 없음
		lock = FALSE;
	else // waiting하는 프로세스가 CS 진입할 수 있도록 함
		waiting[j] = FALSE;
	/*
		Remainder section
	*/
} while(TRUE);
```
##### Swap()
```c
void Swap(boolean *a, boolean *b) {
	boolean temp = *a;
	*a = *b;
	*b = temp;
}
```
###### Swap()을 이용한 해결법
+ Mutual Exclusion 해결 가능
공유 변수를 `boolean lock`으로 설정하고, `false`로 초기화해 사용. 각 프로세스는 지역 변수 `boolean key`를 가짐
```c
do {
	key = TRUE;
	while(key == TRUE) 
		Swap(&lock, &key); // busy waiting. lock이 false가 될때까지 wait
	/*
		Critical Section
	*/
	lock = FALSE;
	/*
		Remainder Section
	*/
} while(TRUE);
```
### [[Semaphore]]
![[Semaphore]]
## [[Monitor]]
--
![[Monitor]]