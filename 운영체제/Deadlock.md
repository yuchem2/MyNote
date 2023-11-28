두 개 이상의 프로세스들이 기다리고 있는 프로세스 중 하나에 의해서 일어날 수 있는 이벤트를 무한 대기하고 있는 상태
## Concepts
Resource를 붙잡고 있으면서 block된 프로세스들의 집합과 resource를 얻기 위해 기다리는 집합들의 집합이 존재하는 상황을 Deadlock Problem이라고 말한다. 

Deadlock 문제를 해결하기 위해 가장 단순한 방법은 Bridge Crossing 예제를 이용하는 것으로, 두 차가 정면에서 마주치게 될 때 한 차가 물러남으로써 Deadlock을 해결할 수 있다. 하지만 이러한 경우에 여러 차가 물러나는 경우가 생길 수 있고, 한 차가 계속해서 물러나는 경우도 생길 수 있다. 즉, Deadlock을 해결할 때 [[Starvation]] 문제가 생길 가능성이 존재한다. 
### System Model
Deadlock 문제를 해결하기 위한 예시를 나열할 때 Resource type의 집합 R과 각 Resource instance 집합 W를 이용해 예시를 나열할 예정이다. 

각 프로세스는 다음과 같은 방법으로 자원을 사용한다. 
+ Request: 요청한 resource의 접근이 바로 시행되지 않으면, resource가 할당될 때까지 wait
+ Use: 프로세스가 resource를 이용
+ Release: 프로세스가 resource 사용을 마치고, resource에 대한 권한을 내려놓음
### [[Resource-Allocatin Graph]]
![[Resource-Allocatin Graph]]
## Characterization
Deadlock은 다음과 같은 4개의 상황이 동시에 발생하는 경우 야기될 수 있다.
+ Mutual exclusion: 오직 한 순간에 하나의 프로세스만이 자원을 사용 가능
+ Hold and wait: 적어도 한 개 이상의 자원을 가지고 있는 프로세스가 다른 프로세스에 잡혀있는 자원을 추가적으로 사용하기를 원하는 상태
+ No preemption: 자원을 가지고 있는 프로세스는 오직 자신의 작업이 끝난 후 자발적으로만 자원에 대한 권한을 내려놓는다
+ Circular wait: 자원을 기다리고 있는 프로세스 집합 $\{P_0, P_1, ... , P_n\}$이 있을 때 각 프로세스가 기다리는 형태가 원형을 이루는 상황을 말함
	+ $P_0$은 $P_1$가 가진 자원을 기다리고, $P_1$은 $P_2$가 가진 자원을 기다리고, ... $P_n$이 $P_0$가 ㅈ가진 자원을 기다리는 형태
### Example
| No Deadlock                          | Deadlock                             |
| ------------------------------------ | ------------------------------------ |
| ![[Pasted image 20231128093644.png]] | ![[Pasted image 20231128093659.png]] |

좌측 그림의 경우 Mutual Exclusion, Hold and wait, No Preemption은 만족하지만, Circular Wait을 만족하지 않기 때문에 언젠가 $P_3$이 $R_3$에 대한 권한을 내려놓으면서 멈춰있는 상황이 해결된다. 

우측 그림의 경우 좌측 그림에서 $P_3$이 $R_2$에 대한 권한을 요청한 경우인데, 이 경우에 추가적으로 Circular Wait이 만족되어 Deadlock 상태라고 볼 수 있다. 
### Basic Facts
[[Resource-Allocatin Graph]]에서 cycle이 존재하지 않는 경우에는 Deadlock 상태가 아니다. 하지만 cycle이 존재하는 경우에 각 자원의 객체가 하나밖에 존재하지 않는 경우에는 Deadlock이 생기고, 하나 이상 존재하는 경우에는 Deadlock에 빠질 가능성이 존재한다. 
## [[Deadlock handling]]
![[Deadlock handling]]
## Recovery from Deadlock
