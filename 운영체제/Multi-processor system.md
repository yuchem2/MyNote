
근접하여 통신하는 두 개 이상의 CPU를 가진 [[Computer System]]

각 CPU는 bus, clock, memory, peripheral device를 공유

## **장점**
+ 처리율(throughput) 증가: 적은 시간에 더 많은 일을 수행할 수 있음
+ Economy of scale: 여러 개의 [[Single-processor system]]보다 비용이 저렴함
+ 신뢰성(Reliability) 증가: 하나의 CPU가 고장나도, 느려질 뿐 시스템이 멈추지 않음

## **단점**
+ 복잡성(complexity) 증가: [[Single-processor system]]보다 HW, SW 적으로 복잡하다


## **종류**
+ Asymmetric: 프로세서가 master-slave 관계를 가짐. master가 slave에게 일을 할당하고, 관리
			  주로 [[입출력]]을 master가 담당
			  
+ Symmetric: 프로세서가 동일한 관계를 가짐. 각 프로세서는 간선 없이 자신의 일을 수행