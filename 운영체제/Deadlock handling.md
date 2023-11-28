Deadlock을 처리하는 방법들을 크게 구분하자면 다음과 같다.
1. Deadlock prvention/avoidnace: 시스템에서 deadlock 상태에 들어가지 않는 것을 보장
2. Deadlock detection/recovery: deadlock 상태에 들어갈 수 있지만, 그 후 회복
3. Deadlock을 무시하고, deadlock이 절대 일어나지 않게 한다.

3번 방법이 현재 대부분의 [[운영체제(Operating System)]]에서 취하는 방법이다. Deadlock 처리를 개발자에게 맡기고 있다. 
## [[Deadlock Prevention]]
![[Deadlock Prevention]]
## [[Deadlock Avoidance]]
![[Deadlock Avoidance]]
## [[Deadlock Detection]]
![[Deadlock Detection]]
## Recovery from Deadlock
[[Deadlock Detection]]이 [[Deadlock]]을 탐지하면, 다양한 대처 방법이 존재한다. 
+ Operator에게 [[Deadlock]]이 일어났음을 알리고, 직접 수동으로 처리하도록 함
+ [[Process Termination]]: circular wait을 제거하기 위해 하나 이상의 프로세스를 종료 시킨다. 
+ Resource Preemption: [[Deadlock]]으로 멈춘 하나 이상의 프로세스들이 자원을 내놓게 함
### [[Process Termination]]
1. 모든 [[Deadlock]] 프로세스들을 종료시킨다
2. [[Deadlock]] circural wait이 제거될 때까지 한 번에 한 개의 프로세스를 종료 시킴. 종료해야 하는 프로세스는 다음과 같은 순서로 선택된다. 
	+ 프로세스 우선순위
	+ 프로세스가 얼마나 실행되었고, 종료될 때까지 얼마나 남았는지
	+ 프로세스가 사용한 자원 수
	+ 프로세스가 완료되기 전까지 필요한 자원 수
	+ 프로세스가 interactive or batch인지
	+ 얼마나 많은 프로세스들이 종료될 필요가 있는지
### Resource Preemption
1. 희생자를 선택([[Process Termination]]) 경우와 동일
2. Rollback: Safe state가 되도록 되돌아가서 그 상태에서부터 프로세스가 재시작
3. [[Starvation]]: 같은 프로세스가 항상 희생자로 선택될 수도 있음
