[[Memory Management]] 방법 중 하나로, 주소 공간을 의미를 가진 논리적인 단위인 segment로 나눠 관리하는 방법이다. 여기서 논리적인 단위는 다음과 같은 값을 가질 수 있다.
+ main program
+ procedure
+ function
+ method
+ object
+ local variables, global variables
+ common block
+ stack
+ symol table, arrays
## User View
이 방법으로 나누게 되면, 프로그램의 메모리의 user [[뷰(View)]]인 segmentation으로 관리할 수 있고, 프로그램을 segments의 조합으로 여긴다. 각 Segment는 그 segment의 시작을 가리키는 offset을 통해 구분될 수 있다. ($<$segment-name, offset$>$)

논리 주소는 segment의 집합이 되고, 각 segment는 고유한 이름과 길이를 가지게 된다. Segmentation은 각 segment를 물리 주소에 매핑하게 된다. 이때 segment의 길이는 모두 다를 수 있다. 
## Architecture
실제 segment는 segment number와 offset의 조합으로 구분될 수 있다. 논리 주소 공간의 segment를 실제 물리 주소 공간에 mapping 하기 위해 segment table이 사용된다. segment table은 segment number를 index로 가지고, base값과 limit 값을 가진다.
+ base: 그 segment가 시작하는 공간의 물리 주소
+ limit: segment의 길이

Segment table은 [[Paging 기법]]의 page table처럼 STBR(Segement-table base register), STLR(Segment-table limit register)에 의해 기록된다. 

[[Paging 기법]]과 동일하게 valdiation bit, protection bit를 추가해 사용할 수 있다. 
### Allocation
Segment의 길이는 상이함으로, memory 할당 과정에서 [[Dynamic Storage-Allocation Problem]]가 생길 수 있다. 
### Relocation
Dispatcher에 의해 Swapping이 발생할 수 있다. 동적으로 발생하며, segment table에 의해 관리되어야 한다.
### Sharing
Segment는 [[Paging 기법#Shared Pages]]와 동일하게 공유될 수 있다. 
