
[[운영체제(Operating System)]]가 협력하는 프로세스들 간에 메시지를 전달해 줌으로써 통신하는 시스템

[[운영체제(Operating System)]]가 작업에 도움을 주기 때문에 일반적으로 많이 사용한다

[[운영체제/Process]]는 network로 연결된 서로 다른 컴퓨터에 존재할 수 도 있으며 공유 변수에 의지하지 않고 서로 통신한다

각 pocess들 간의 통신을 위해서는 통신 링크가 필요하고 이 통신 링크를 signal, pipe, socket, rpc, rmi 등의 방법으로 구현할 수 있다

## Operation([[System Call]]에 의해 실행)
+ send(message): size fixxed or variable
+ recevie(message): 


## 구현 방식
+ Direct or Indirect
	+ [[Direct Communication]] 
	+ [[Indirect Communication]]
+ Synchronous or Asynchronous: 메시지 전송이 blocking이나 non-blocking 둘 중의 하나로 전달된다
	+ Synchronous(blocking): 메시지가 배달될 때까지 송신자가 차단, 가져갈 메시지가 생길 때까지 수신자가 차단
	+ Asynchronous(non-blocking): 송신자가 메시지를 계속해서 보낼 수 있고, 수신자 또한 메시지가 있으나 없으나 수신이 가능하다
+ Autonomic or Explicit buffering: message queue를 만들어 통신 링크로 사용하는 방법으로 큐를 구현하는 방법은 다음과 같다
	+ Zero capacity: 큐에는 0개의 메시지가 존재. 최대크기가 0. 송신자는 수신자가 메시지를 받을 때까지 기다린다
	+ Bounded capacity: 일정 길이의 n개의 메시지. 송신자는 link가 차있다면 기다린다
	+ Unbounded capacity: 무제한 길이. 송신자는 절대 기다릴 필요가 없다