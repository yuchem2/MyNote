## Hole
유효한 메모리 블럭을 말하며, 메모리 할당 과정에서 메모리 부분에서 할당된 공간 사이에 빈 공간을 말한다. 만약 이 공간이 충분히 크다면, 새로 들어온 프로세스를 그 공간에 할당 가능하다. 

구멍들에 할당할 때는 n 크기의 요청에 따라 여러 할당하는 방법이 존재한다. 
+ First-fit: 충분히 큰 첫 번째 hole에 할당
+ Best-fit: 충분히 큰 hole 중 가장 작은 hole에 할당
	+ 크키 순서대로 정렬되어 있지 않으면 전체 리스트를 모두 확인해야 함
+ Worst-fit: 가장 큰 hole에 할당한다.
	+ 이 방법도 best-fit과 동일하게 정렬되지 않으면 모두 확인해야 함

First-fit과 best-fit이 worst-fit에 비해 속도와 사용률 측명에서 좋다. 
## Fragmentation
메모리를 할당하다 보면 필연적으로 빈 fragmentation이 생긴다. 
+ External: 전체 메모리 공간은 한 요청을 만족 시키기엔 충분하나 연속적이지 않음
+ Internal: 할당된 메모리가 요청한 메모리보다 아주 약간 작은 경우

이러한 경우에 compaction을 통해 external fragmentation을 줄인다.
+ 모든 free memory space가 함께 모여 큰 하나의 블럭으로 만들기 위해 메모리 내용을 섞음
+ 오직 relocation가 동적일 때만 가능하다. 또한 execution time에 이뤄진다.
+ 입출력 문제가 존재하고 이를 해결하는 방법은 다음과 같다.
	+ 입출력에 포함되는 동안은 메모리에 작업이 잠굼
	+ 입출력은 단지 OS buffer를 이용해 수행