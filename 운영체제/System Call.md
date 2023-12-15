
[[운영체제(Operating System)]]가 지원하는 서비스에 대한 Programming Interface

[[운영체제/Process]]가 [[운영체제(Operating System)]]와 통신하는 유일한 방법

전통적으로 [[고급 프로그래밍 언어]]인 [[C]] 나 [[C++]]로 작성되며 때때로 [[어셈블리 언어(Assembly Language)]]로도 작성된다

대부분 프로그램에 의한 접근은 직접적인 호출보다 [[API(Application Program Interface)]]을 통해 수행된다
+ API를 사용하는 이유는 더 간단한 방법을 제공하고, 구현에 대한 자세한 내용을 숨기기 위함

## 예시
![[Pasted image 20230927000819.png | 600]]

## 구현
전통적으로 각 호출에는 번호가 할당되어 구현된다
+ 이 번호를 index로 하여 table을 만들어 관리한다
+ [[운영체제(Operating System)]] kernel 내의 의도하는 system call을 호출하고, 상태와 반환 값을 프로그램으로 돌려준다

호출자, 즉 사용자는 어떻게 구현되는지에 대해서는 알 필요가 없고, [[API(Application Program Interface)]]를 준수해 결과가 어떻게 반환되는지에 대해서만 이해하면 된다



## Parameter Passing
일반적으로 세가지 방법이 존재한다
1. Simplest: parameter를 [[레지스터(Register)]] 내에 전달하는 것
2. Block: [[Main Memory]]내의 블록이나 테이블에 저장하고, 그 주소를 [[레지스터(Register)]]에 전달
3. [[스택(Stack)]]]: 프로그램이 paremeter를 push하고, [[운영체제(Operating System)]]에 의해 pop됨

[[운영체제(Operating System)]]는 Block과 Stack방법을 선호함. (개수 및 길이에 제한 x)


## 종류
+ [[운영체제/Process]] control
+ File Management
+ Device Management
+ Information maintenance
+ Communications