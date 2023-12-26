Physical memory로부터 유저의 logical memory를 분리하는 것으로 다음과 같은 특징을 가진다.
+ 실행을 위해 프로그램의 일부만 메모리에 저장할 수 있다. 
+ Physical address space보다 logical address space가 더 클 수 있다.
+ 몇 개의 [[운영체제/Process|Process]]들 간에서 주소 공간이 공유될 수 있다.
+ 조금 더 효율적인 [[Process Creation]]을 허용한다.

Demand [[Paging 기법]], Demand [[Segmentation]] 기법을 통해 구현할 수 있다.
