> Illusion of a computer system with own CPU, memory, and I/O

가상의 [[운영체제(Operating System)]]을 만들어 현재 컴퓨터에 설치된 [[운영체제(Operating System)]] 위에서 응용프로그램 형태로 가동할 수 있도록 구현한 프로그램

계층화 접근 방법을 이용하고, 하드웨어와 os kernel을 하드웨어인 것처럼 취급한다

기본 베어 HW와 동일한 인터페이스를 제공

물리적인 컴퓨터와 VM은 서로 resource를 공유

하나 이상의 VM을 하나의 물리적인 컴퓨터에 생성할 수 있다

각 VM은 독립적으로 실행되며 각 리소스에 대한 permission도 상이할 수 있다

[[운영체제(Operating System)]] 연구나 개발을 위해 자주 사용된다