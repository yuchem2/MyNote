Producer가 Consumer가 소비하는 정보를 생성만 하고, Consumer는 정보를 소비만 하는 상태

즉, 두 프로세스가 동시에 수행되지만, 한 쪽에서는 생성만 하고 소비만 하는 상태이다. 

### Memory
```c
#define BUFFER_SIZE 10
typedef struct {
	int content;
} item;

item buffer[BUFFER_SIZE];
int in = 0;
int out = 0;
```
+ BUFFER_SIZE - 1 만큼 데이터 저장 가능
+ Empty: in == out
+ Full: (in+1)%BUFFER_SIZE == out
### Insert
```c
void insert(void) {
	item nextProduced;
	
	// Produce an item in nextProduced
	while (((in + 1) % BUFFER_SIZE) == out) ;
	// do nothing -- no free buffers 

	// insert an item into the buffer
	buffer[in] = nextProduced;
	in = (in + 1) % BUFFER_SIZE;
}
```
### Remove
```c
item remove(void) {
	item nextConsumed;

	while(in == out) ;
	// do nothing -- nothing to consume

	// remove an item from the buffer
	nextConsumed = buffer[out];
	out = (out + 1) % BUFFER_SIZE;
	return nextConsumed;
}
```