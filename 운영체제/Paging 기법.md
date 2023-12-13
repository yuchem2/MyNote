[[Memory Management]] 방법의 일종으로, 다음을 가능케 한다.
+ 프로세스의  physical 주소 공간은 연속적이지 않아도됨
+ 프로세스는 메모리가 유효할 때 언제든지 물리 메모리에 할당됨

paging 기법을 사용할 때 physical memory는 frame(512 bytes)로 나뉘게 된다. Logical 주소는 frame과 동일한 크기의 블록으로 나누고, 이를 page라고 부른다.

모든 자유 frame을 기록하고 관리한다. Logical 주소를 physical 주소로 변환하는 page table을 설정한 후 이를 통해 page와 frame을 매핑함으로써 할당한다. 이 경우에 external fragementation은 생기지 않지만 internal fragmentation은 생긴다. 
## Address Translation Scheme
CPU에 의해 생성되는 logical 주소는 다음과 같은 요소로 구성된다.
+ Page number (p): physical memory 내의 각 page의 기본 주소를 포함하는page table의 인덱스로 사용된다.
+ Page offset (d): 메모리 유닛에 보내지는 physical memory 주소를 정의하기 위해 base address와 조합된다.

Logical address space = $2^m$, size of page = $2^n$인 경우 다음과 같이 구성된다. 
+ 상위 m-n bit는 page number(p)를 지정.
+ 하위 n bit는 page offset(d)를 지정

CPU에서 page number + page offset이 입력되면 page number를 통해 page table에서 frame number를 찾는다. page number를 frame number로 변경시킨 후 이를 통해 physical memory에 접근한다. 
### Page Table Size
page table의 크기는 logical address space와 page table enrty에 의해 결정된다. 일반적으로 page number는 4 bytes, frame의 크기는 4($=2^{12}$) KB이다. 이렇게 결정한다면 이 시스템은 $2^{44}$ bytes의 주소를 지정할 수 있다. 
### Free Frames
새로운 프로세스가 생성되면, 운영체제는 필요한 page 개수 만큼의 할당 가능한 frame이 있는지 확인한다. 할당이 가능하다면 각 page마다 frame을 할당한다.
## Implenation of Page Table
Page table은 main memory에 저장되어 있다. 
+ PTBR(Page-table base register): 페이지 테이블을 가리키는 레지스터
+ PRLR(Page-table length register): 페이지 테이블의 길이 정보를 저장

모든 데이터/명령 접근은 두 번의 메모리 접근이 필요(페이지 테이블, 데이터/명령어 접급). 이러한 문제는 TLB(tranlation look-aside buffers)로 해결 가능하다.
### TLB
특수한 빠른 검색이 가능한 H/W로, page table을 위한 cache로서 사용된다.
TLB는 몇 개의 page table entry 정보를 가지고 있다. 다음과 같은 과정으로 TLB는 logical 주소에 대한 처리를 수행한다. 
1. CPU에 의해 logical 주소가 생성되면 그 page 번호를 TLB에 제시한다.
2. 만약 그 page number를 TLB에서 찾으면 frame number를 바로 알 수 있어 바로 메모리에 접근하면 된다.
3. 만약 찾지 못하는 것을 TLB miss라고 하며 이럴 땐 직접 page talbe에 접근해 frame number를 찾는다.
4. 이렇게 찾은 frame number를 TLB에 page number와 함께 추가한다.
5. 만약 TLB 항목이 가득 차 있는 경우에 OS는 이를 교체하기 위해 하나를 선택한다. 이때 선택되는 부분은 가장 최근에 사용되지 않은 것(LRU) 혹은 무작위이다. 
#### Effective Access Time 
적중률($\alpha$)를 page number가 TLB에서 발견될 확률로 정의한다. TLB 검색 시간을 $\epsilon$로, 메모리 접근 시간을 $\tau$로 가정하면 유효 접근 시간(EAT)를 다음과 같이 정의할 수 있다. $$EAT=(\tau+\epsilon)\alpha + (2\tau+\epsilon)(1-\alpha)$$
### Memory Protection
각 frame에 연관된 protection bit를 추가함으로써 구현할 수 있다. 
+ one bit 값이 사용되며 page가 읽기 전용인지 쓰기 가능인지 정의 가능
+ 읽기 전용 page에 쓰기 시도는 H/W trap을 OS에게 보내 읽기를 방지한다.

page table의 각 항목에 Valid-invalid bit를 추가함으로써 관련된 페이지가 합법적인지 여부를 확인할 수 있다.
+ Valid: 관련된 page가 프로세스의 logical 주소 공간에 존재하며 합법적인 page임을 나타냄
+ Invalid: page가 프로세스의 logical 주소 공간에 속하지 않는다는 것을 나타낸다. 
## Page Table Structure
### Hierarchical Paging
32 bit 크기의 논리 주소 공간을 사용하고, 4 KB의 page를 사용하는 시스템의 경우 page table의 크기는 $(2^{32}/2^{12})\times 4$ byte$=4$ Mbytes가 된다. 크기가 매우 큰 테이블이 생성되는 것을 볼 수 있다. 이를 해결하기 위해 논리 주소를 여러 개의 page table로 나누는 방법이 요구되었다.

가장 간단한 방법은 두 개의 계층을 만드는 방법이다.
#### Two-level Paging Example
위와 같은 예시에서 일반적인 paging 기법을 사용하는 경우 page number는 20 bit로 표현될 수 있고, page offset은 12 bit로 표현된다. 이 상태에서 page number를 다시 한번 10 bit의 page number, 10 bit의 page offset으로 나누게 되면, 논리 주소는 다음과 같아진다. 
+ page number $p_1 = 10\; bit$
+ page number $p_2 = 10\;bit$
+ page offset $d=12\;bit$

$p_1$은 외부 page table의 index가 되고, $p_2$는 외부 page table의 값을 통해 실제 page로 바꿔주는 역할을 수행한다. 이 경우에 3번의 메모리 접근이 필요해 성능은 상대적으로 감소할 수 있다. 하지만 page table의 크기를 최소화할 수 있다.
### Hashed Page Table
virtual page number를 [[Hash Function]]을 이용해 page table에 접근한다. 즉, page table을 [[Hash Table]]로서 사용한다. 이때 chaining 기법을 사용한다. 각 element는 다음과 같은 값을 가진다. 
+ virtual page number
+ value of the mapped page frame
+ a pointer to the next element
### Inverted Page Tables
page table을 page number만으로 찾는 것이 아니라, pid를 함께 사용해 entry로 사용한다. 동적 배열로 page table을 유지해 메모리 공간을 최소화할 수 있다. 하지만 프로세스 수가 증가할 수록 탐색 시간이 증가한다. 

### Shared Pages
Paging을 활용하는 또 다른 장점으로, code sharing이 용이해진다. process끼리 공유하는 영역을 따로 공유할 필요 없이 공유하는 부분을 page로 만들어 같은 frame을 가리키도록 만들 수 있다. 
#### Shared Code
Read-only code 부분을 공유하도록 만들고, 여러 프로세스들의 해당 page 번호가 같은 frame을 가리키도록 만든다. 이때 각 프로세스의 논리 주소에서 같은 공간에 등장하도록 만든다.  
#### Private code and data
각 프로세스는 자신의 고유의 code와 data를 유지한다. 

