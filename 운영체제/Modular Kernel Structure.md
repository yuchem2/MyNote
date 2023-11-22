
Kernel을 핵심 구성 요소(core components)로 나누어 부팅시나 싫생시에 부가적인 서비스들을 동적으로 연결한 [[운영체제(Operating System)]]

Object-oriented 접근을 사용하며 각 핵심 구성 요소는 나뉘어서 존재
+ 각 구성 요소는 정해진 인터페이스를 통해 다른 구성 요소와 통신
+ 각 구성 요소는 커널 내에서 필요에 따라 적재 가능

[[Layered Structure]]와 유사하지만 좀 더 유연한 형태(각 모듈은 제한 없이 서로 접근 가능)