Circular-wait 조건이 일어나지 않는 것을 동적으로 검사함으로써 [[Deadlock]]을 회피하는 방법. 
## Resoure Allocation State
자원 할당 상황은 사용 가능한 자원의 수, 할당된 자원 수, 프로세스의 최대 요구량으로 정의된다. 자원 할당 상황이 가질 수 있는 상태는 safe, unsafe이다. Safe는 circular-wait이 없는 상태이고, unsafe는 circular-wait이 존재해 [[Deadlock]]이 생길 수 있는 상태를 말한다. 
## Algorithm
### Basic Concepts
safe sate인 경우에 [[Deadlock]]이 발생할 가능성이 존재하지 않지만 unsafe state는 [[Deadlock]] 발생할 가능성이 존재한다. 그러므로 [[Deadlock Avoidance]]에서는 절대 unsafe sate에 진입하지 않도록 한다. 
### Safe State
프로세스가 사용 가능한 자원을 요청할 때 시스템은 그 자원을 할당하더라도 safe state가 유지되는 지 확인한다. 모든 프로세스의 safe sequence가 존재하면 현재 safe sate가 유지되고 있음을 보장한다. 

Safe Sequence $\{P_1, P_2, ..., P_{i-1}, P_i, P{i+1}, ..., P_n\}$는 $i<j$일 때 각 $P_i$는 현재 사용 가능한 자원과 $P_j$가 가지고 있는 자원을 요청할 수 있다는 것을 의미한다. 
### [[Resource-Allocatin Graph]] Alogrithm
먼저 기존 [[Resource-Allocatin Graph]]에서 새로운 간선 종류인 claim을 추가한다. 이 간선은 프로세스가 자원을 요청할 수 있음을 나타내며 점선으로 표현한다. 이 간선이 추가되면 프로세스가 자원을 요청하는 전체 과정은 claim $\rightarrow$ request $\rightarrow$ allocation 순으로 간선이 변화되며 나타난다. 

$P_i$가 $R_j$를 요청한다고 가정할 때 request edge $P_i \rightarrow R_j$를 allocation edge $R_j \rightarrow P_i$로 바꾸는 경우에만 요청을 허용 가능하게 해 [[Resource-Allocatin Graph]]에서 cycle이 형성되지 않도록 한다. 

이 알고리즘은 여러 개의 instance를 가진 resource type을 지닌 시스템에는 적용 불가능하다.
#### Example
| Safe                                 | Unsafe                               |
| ------------------------------------ | ------------------------------------ |
| ![[Pasted image 20231128105240.png]] | ![[Pasted image 20231128105246.png]] |

왼쪽의 경우에 safe sequence가 $\{P_1, P_2\}$로 만들어지며 cycle이 존재하지 않는다. 오른쪽 경우에서는 $P_1 \rightarrow R_2 \rightarrow P_2 \rightarrow R_1 \rightarrow P_1$의 cycle이 형성된다. 그러므로, $P_2$가 $R_2$를 할당 받지 못하게 만들어야 한다. 
### Banker's Alogrithm
2개 이상의 instance를 지닌 자원 유형을 처리하기 위한 [[Deadlock Avoidance]] Alogrithm. [[Resource-Allocatin Graph]] 방법보다는 비교적으로 덜 효율적인 방법이다. 

이 알고리즘에서는 다음과 같은 상황을 가정하고 있다. 
+ 각 프로세스는 사용을 자신이 사용할 자원에 대해 claim을 만들어야 한다.
+ 프로세스가 자원을 요청할 때 기다려야 할 수 있다.
+ 프로세스가 필요한 모든 자원을 얻으면 정해진 유한 시간 내에 자원을 내려놓아야 한다.
#### Data Structures
$n$은 프로세스의 수, $m$은 자원 유형의 수를 말하며 다음과 같은 데이터 구조를 가짐
+ Available: 길이가 $m$인 vector, 각 자원 유형에서 사용 가능한 instance의 수
+ Max: $n\times m$ matrix, 각 프로세스가 요청할 수 있는 각 자원 유형의 최대 instance 수
+ Allocation: $n\times m$ matrix, 각 프로세스가 현재 소유한 각 자원 유형의 instance 수
+ Need: $n\times m$ matrix, 각 프로세스가 필요로 하는 각 자원 유형의 instance 수
+ $Need[i,j] = Max[i,j] - Allocation[i, j]$
#### Safety Algorithm
safe/unsafe를 판단하는 알고리즘.
```pseudo code
Let Work[1...m] = Available, finish[1...n]
finish[i] = fasle for i=0 to n

repeat
if finish[i] == false && Need[i] <= Work
	Work = Work + Allocation[i]
	finish[i] = true
i++
until no such i exsits

if finish[i] == true for all i
	return safe
else
	return unsafe
```
#### Resource-Request Alogrithm
safety algorithm을 통해 자원할당을 결정하는 알고리
```pseudo code
Let request = request vector of process Pi

if request <= need[i]
	if request <= Available
		Available = Available - request
		Allocation[i] = Allocation[i] + request
		Need[i] = Need[i] - request

		if safety alogorithm == safe:
			resource allocate to Pi
		else
			Pi wait, and the old resource-allocation state is resotred
	else
		Pi wait
else
	return error(exceeded maximum claim)

```
