[[Deadlock]] 상태에 진입하는 것을 허용한다. [[Deadlock]] 상태를 탐지해 현재 [[Deadlock]] 상태이면, 회복 상태로 진입하며 [[Deadlock]]을 해결한다. 
## Algorithm
### Single Instance
[[Resource-Allocatin Graph]]을 wait-for graph로 바꾸어 이용한다. wait-for graph에서는 자원 정점을 제거해 프로세스 노드만이 존재한다.

![[Pasted image 20231128113935.png | 600]]

주기적으로 cycle이 존재하는지 확인하는 알고리즘을 실행시켜 [[Deadlock]]을 탐지한다. 일반적으로 그래프에서 cycle이 존재하는지 확인하는 알고리즘은 $O(n^2)$이 소요되며 이때 $n$은 정점의 수이다. 
### Several Instance
[[Deadlock Avoidance#Banker's Alogrithm]]을 활용한 형태로, 다음과 같은 데이터 구조를 사용한다. 
+ Available: 길이가 $m$인 vector, 각 자원 유형에서 사용 가능한 instance의 수
+ Allocation: $n\times m$ matrix, 각 프로세스가 현재 소유한 각 자원 유형의 instance 수
+ Request: $n\times m$ matrix, 각 프로세스가 현재 요청하는 각 자원 유형의 instance 수

```pseudo code
Let work[1...m] = Available, finish[1...n]
for i = 0 to n-1
	if Alloaction[i] != 0 
		finish[i] = false
	else
		finish[i] = true

repeat
if finish[i] == false && Request[i] <= work
	work = work + Allocation[i]
	finish[i] = true
until no such i exists

for i = 0 to n-1
	if finish[i] == false
		return Pi is deadlocked
```
이 알고리즘은 $O(m\times n^2)$이 소요된다.
### Usage
두 알고리즘에서 볼 수 있듯이 모두 많은 시간을 소요하는 알고리즘이다. 그러므로 탐지 알고리즘을 언제, 어느 주기로 실행시킬지 결정하는 것이 중요하다. 만약 이러한 설정 없이 탐지 알고리즘을 실행하고 싶을 때마다 수행한다면 어느 프로세스가 [[Deadlock]]을 유발했는지 확인하기도 어려워 회복하기 어려운 문제가 생길 수 있다
