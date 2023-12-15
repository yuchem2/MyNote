[[운영체제/Process]]가 실행되기 위해 모든 데이터는 메모리에 존재해야 한다. 실행하기 위해 모든 명령어들도 메모리에 있어야 한다

[[운영체제(Operating System)]]는 Memory Management를 통해 언제, 무엇이 메모리에 있어야 하는지를 결정한다. 이는 CPU 사용률과 사용자에 대한 컴퓨터 반응 시간에 대한 최적화에 의해 결정된다
## Activities
+ 메모리의 어떤 부분이 누구에 의해 사용되는지 추적, 확인한다
+ 어떤 [[운영체제/Process]]가 데이터를 메모리 상에 올리거나 내리는 것을 결정한다
+ 필요에 따라 메모리 공간을 할당하고, 회수한다
## Background
CPU scheduling의 결과로 CPU 사용율, 컴퓨터의 반응 속도가 향상된다. 이러한 성능의 향상을 인식하기 위해 메모리에 여러 개의 프로세스를 유지해야 한다. 

프로그램은 메모리로 옮겨져서 실행되기 위해 프로세스 내에 위치하게 된다. 이러한 프로세스는 다음과 같은 성질을 가진다. 
+ 각 프로세스는 분리된 메모리 공간을 가진다.
+ Base register와 limit register는 물리 메모리 내의 프로세스 주소 공간을 결정하는데 사용된다.
	+ Base register: 가장 작은 물리 메모리 주소를 가짐(시작 주소)
	+ Limit register: 프로세스 주소 공간의 크기를 저장한다. 
	+ 이 값들은 PCB에 저장된다. 
### H/W address protection
메모리 공간 보호는 CPU가 user mode에서 생성되는 메모리 주소를 레지스터와 비교함으로써 이뤄진다. 
모든 불법적인 메모리 접근은 운영체제에게 trap을 통해서 알린다.$$base\; register \leq address < base\; register + limit \; register$$
### Input Queue
입력 큐는 디스크 상의 프로세스들이 실행되기 위해 메모리로 옮겨지기를 기다리고 있는 집

프로그램은 디스크에 이진 파일로 저장되어 있다. 
+ 실행되기 위해 프로그램은 메모리로 옮겨져 프로세스 내에 위치해야 한다.
+ 사용되는 [[Memory Management]] 방법에 따라, 프로세스는 실행되는 동안 디스크와 메모리 사이를 이동하게 된다.
+ 디스크 내의 프로세스는 실행되기 위해 Input queue로부터 메모리를 옮겨지기를 가다리고 있다.
### Multi-step Processing of  a User Program
User program은 실행되기 전에 몇 가지 단계를 거친다. 주소는 이 단계 동안에 명령어나 자료로 표현될 수 있다. 주소는 일반적으로 컴파일, 로딩, 실행 타임에 결정된다. 단계는 다음과 같다.
1. source program
2. compile time: compiler or assembler
3. object module
4. load time: object module + other object modules -> Linkage editor -> load module + system library -> combine by loader
5. excution time: dynamic loaded system library + In-memory binary memory image 

source program의 주소는 generally symbolic(int A;)로 나타낸다. [[컴파일러(Compiler)]]는 전형적으로 이 주소를 재배치 주소로 바인드(14 byte가 이 함수의 시작을 형성). linkage editor나 loader는 이 재배치 주소를 절대 주소로 바인드
### Address Binding
명령어와 데이터의 메로리 주소에 대한 주소 바인딩은 다음과 같은 단계에서 이루어진다. 
1. Compile time: 메모리 위치를 사전에 알고 있다면, absolute code가 생성된다. (항상 같은 공간에 upload됨) 만약 시작 위치가 바뀌면 코드를 다시 컴파일 해야 한다. 
2. Load time: 메모리 위치가 compile time에 알려지지 않았다면 relocatable code를 생성해야 함
3. Execution time: 프로세스가 실행되는 동안 하나의 메모리 세그먼트에서 다른 세그먼트로 옮겨질 수 있다면 바인딩은 실행시간까지 미뤄짐. 주소 맵을 위한 H/W 지원이 필요(e.g. base/limit register)

### Logical vs. Physical Address Space
올바른 [[Memory Management]]의 핵심은 분리된 physical 주소 공간에 바인딩 된 logical 주소 공간의 개념이다. 
+ Logical address: CPU에 생성되며, virtual address라고도 한다.
+ Physical address: 메모리 유닛에서 사용되는 주소

논리 및 물리 주소는 compile time에서는 바인딩과 같은 방법으로 사용된다. 하지만 load/execution time에서는 바인딩 방법과 다른 방법을 사용한다.
#### MMU(Memory-Management Unit)
logical 주소를 physical 주소에 매핑시키는 H/W 장치. MMU에서 relocation register의 값이 메모리로 보내지는 순간에 사용자 프로세스에 의해 생성되는 모든 주소에 그 값이 더해진다. $$logical \; address + base \; register = physical \; address$$user program은 logical 주소를 사용하며, 실제 physical 주소에 대해서는 알지 못한다. 이러한 방법을 dynamic relocation라고 한다. 
### Dynamic Loading
필요할 때 데이터, 코드를 메모리에 올리는 방식으로, 프로그램이 실행될 때 작업이 호출될 때까지 메모리에 적재되지 않는다. 사용되지 않는 작업은 메모리에 적재되지 않기 때문에 향상된 메모리-공간 사용율을 보인다. 

자주 사용되지 않는 크기가 큰 코드가 있는 경우에 유용하다. [[운영체제(Operating System)]]의 특별한 지원이 필요하지 않아 user가 자신의 프로그램이 이 방법의 장점을 사용하도록 설계가 가능하다. [[운영체제(Operating System)]]은 dynamic loading을 지원하는 작업을 제공함으로써 프로그래머를 도울 수 있다.
### Dynamic Liking
Linking이 execution time까지 연기 된 후 exeuction time에서 linking된다. 이는 dynamically liked libray에 의해 지원된다. 

작은 코드 부분은 stub이라고 하며, 적절한 memory-resident library routin을 위치시키는 데 사용된다. stub은 자신을 작업 주소와 교체하고 그 작업을 실행한다. 

이 방법은 다중 프로레스들에 의해 공유되는 시스템 라이브러리를 사용할 때 특별히 유용한 방법이다. 
### Swapping
프로세스는 임시적으로 메모리에서 저장 장소(e.g. disk)로 옮겨질 수 있다. 그 후 나중에 실행을 계속하기 위해 다시 메모리로 옮겨질 수 있다. 
+ Baking store: 모든 사용자를 위한 모든 메모리 이미지를 수용할 정도로 충분히 크고, 빠른 디스크. 이 메모리 이미지에 대해 직접 접근을 제공해야 한다.
+ Roll out/in: 우선순위 기반의 [[Scheduling Algorithm]]을 위해 다양하게 사용되는 swapping. 낮은 우선순위 프로세스는 높은 우선순위 프로세스가 적재되어 실행될 수 있도록 Swap out된다.

Swaping 시간의 대부분은 전송 시간이다. Total transfer time은 총 메모리 양에 직접적으로 비례한다. 
### Contiguous Allocation
Main memory는 두 개의 부분으로 나뉘게 된다. 
+ Resident [[운영체제(Operating System)]]: 보통 [[인터럽트(Interrupt)]] vector와 함께 하위 메모리에 위치
+ User processes: 상위 메모리에 위치

연속적인 할당에서는 각 프로세스는 메모리의 하나의 연속적인 부분에 포함된다. Relocation-register 방법은 user process들이 서로를 보호하고, 운영체제 데이터와 코드를 바꾸지 못하도록 보호하는 데 사용한다. 
 + relocation register는 가장 작은 물리 주소 값을 저장한다
 + limit register는 논리 주소 범위를 포함한다. 

즉, 다음과 같이 쓸 수 있으며 MMU는 rerocation register의 값을 더함으로써 논리 주소를 동적으로 매핑하여 메모리에 보낸다. $$logical \; address \; < limit\; register\; \rightarrow \; logical \; register + relocation \; register$$
![[Dynamic Storage-Allocation Problem]]
## [[Paging 기법]]
![[Paging 기법]]
## [[Segmentation]]
![[Segmentation]]