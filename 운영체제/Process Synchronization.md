## Background
---
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
---
![[Race Condition]]
## [[CS(Critical-Section) Problem]]
---
![[CS(Critical-Section) Problem]]