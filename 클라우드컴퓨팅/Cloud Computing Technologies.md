Cloud service는 다양한 기술을 통해 개발되었다. 주 기술은 아래와 같다.
+ Server virtualization technology
+ Container technology
+ Distributed processing technology
+ Database technology
+ Storage technology
+ ...etc
### Virtualization
Cloud computing 환경에서 가상화 기술은 필수적인 기술로, 하드웨어 자원(서버, CPU, 메모리, 저장장소 등)을 논리적으로 처리하는 메커니즘을 말한다. Cloud computing에서 유동적인 대여, 반납, 증설, 축소 등을 가능하게 하는 핵심 기술이다.

1. 하나의 물리적 서버 자원을 여러 개의 서버 환경으로 분할하여 설정할 수 있다. 
2. 여러 물리적 서버 자원을 하나의 서버 환경으로 통합할 수 있다
3. Cloud에서 시스템 구성을 빠르고 유연하게 변경할 수 있고, 자원이 부족할 경우 자동으로 자원을 추가할 수 있다.

가상화 기술은 크게 Server, Network, Storage로 타입을 나눌 수 있다.
#### Server Virtualization
하나의 물리적인 서버를 독립적인 여러 개의 서버로 분리하는 기술을 일컫는다. 물리적인 서버는 종종 자원을 완전히 활용하지 못해 사용되지 않는 용량이 남아있는 문제가 존재한다. 이러한 한계를 해결할 수 있다.
1. 서버 가상화는 여러 개의 개별 물리적 서버를 통합해 서버 자원의 활용도를 극대화할 수 있다.
2. 물리적 서버 수를 줄임으로써 물리적 공간 절약 및 비용 절감을 할 수 있다. 또한, 물리적 서버 구축 시간도 감소한다.
3. 각 가상 서버는 독립적으로 작동하므로, 하나의 가상 서버가 바이러스와 같은 위험에 노출되어도 다른 가상 서버에 영향을 주지 않는다.

![500](Pasted%20image%2020241023141803.png)

서버 가상화 기술도 어떤 방식으로 가상화를 진행하는 지에 따라 3가지 타입으로 구분할 수 있다. 
+ Hypervisor Type: Hypervisor이라는 가상화 소프트웨어를 단일 물리적 서버 하드웨어에서 실행하고, 그 위에 여러개의 Guest OS를 실행하는 방식
	+ 각 분할된 서버의 처리능력은 일반적으로 물리 서버보다 낮음. 
	+ Vmware vSphere, Hyper-V, Xen, KVM, 등 
+ Hosted OS Type
+ Container Type

![500](Pasted%20image%2020241023141938.png)
#### Network Virtualization
Cloud를 구현하기 위해 물리적 구성에 구애받지 않는 유연성이 네트워크에도 요구된다. 이러한 기술에는 VLAN, VPN, NFV 기술 등이 존재한다.
##### VLAN(Virtual LAN)
하나의 물리적 네트워크를 여러 개의 논리적 네트워크로 분할하는 기술. 물리적 배선을 변경하지 않고 네트워크 장치에 설정을 추가해 네트워크를 분할한다. 논리적으로 분할된 네트워크는 라우터를 통하지 않고서는 서로 통신을 할 수 없다. 이를 통해 조직 내에서 네트워크를 단위로 나누어 데이터를 조직 내로 제한하여 전송할 수 있다. Cloud 서비스와 데이터 센터 간에 VLAN을 사용하면 개인 환경을 만들 수 있다.
##### VPN(Virtual Private Network)
인터넷과 같은 불특정 다수가 사용하는 네트워크에 전용선과 유사한 가상 사설망을 연결하는 기술. Cloud 서비스와 기업 사용자가 소유한 on-premise 시스템이 인터넷을 통해 VPN에 연결될 때 IPsec이라는 프로토콜이 사용된다. IPsec을 사용한 통신은 종단점 인증과 데이터 암호화를 통해 종단점 간의 안전한 통신을 보장한다.
##### NFV(Network Functions Virtualization)
네트워크 기능을 소프트웨어로 구현하고 이를 가상 서버에 배포하는 기술. 라우터, 게이트웨이, 방화벽, 로드 밸런서와 같은 네트워크 장치의 기능을 가상 서버에서 애플리케이션 서버로 구현한다. (기존에 전통적인 네트워크에서는 대부분의 네트워크 기능이 전용 하드웨어에 통합되어 있는 상태였다.) 전용 하드웨어 없이 네트워크 기능을 제공하며, 하드웨어 장비를 교체하거나 네트워크 장비의 수요 및 구성을 유연하게 대응할 수 있다.
### Container
가상화 기술의 다른 형태로, 단일 OS 환경에서 실행되는 애플리케이션 공간을 독립적인 여러 개의 공간으로 만드는 기술이다. 이렇게 분할된 공간을 Container라고 한다. Host OS 관점에서 각 container는 개별 프로세스로 인식된다. Cloud에서 container는 빠른 설치, 제거에 이점이 있고, 다른 서비스에서 전환이 쉽다. 

서버 가상화와 비교했을 때 장점은 다음과 같다.
+ 서버 가상화는 전체 하드웨어 환경을 가상화 하지만, 컨테이너는 애플리케이션의 실행 환경을 가상화
+ 컨테이너는 가상 서버에 비해 가상화 overhead가 거의 없기 때문에 시작과 종료 속도가 빠르고, 성능 저하가 없음
+ 컨테이너는 Guset OS가 필요없기 때문에 보조 저장장치 사용량을 줄일 수 있음
+ 개별 컨테이너는 하드웨어 자원을 적게 소모하여 하나의 물리 서버에 많은 수의 컨테이너를 호스팅 할 수 있음

대표적인 Container를 지원해주는 소프트웨어는 Docker와 Kubernetes가 있다.
### Distributed Processing
대량의 데이터를 여러 서버에 분산시켜 동시에 병렬로 효율적으로 데이터를 처리하는 기술을 말한다. 빅데이터 분석처럼 처리해야 할 데이터 양이 많은 경우 cloud computing은 분산 처리를 통해 처리 속도를 가속화할 수 있기 때문에 중요한 기술이다.

Cloud가 등장하기 이전까지 TB, PB 규모의 대용량 데이터를 처리하기 위해 고속 CPU와 대용량 메모리를 지닌 서버가 필요했다. 하지만, 분산 처리 기술과 cloud를 통해 데이터를 여러 서버에 분할하여 병렬로 처리할 수 있다. 또한, 처리 부하에 따라 자원을 확장하거나 축소하는 것이 가능해 서버를 추가하거나 줄이는 것과 유사하게 운영할 수 있게 되었다. 그 결과 대량의 데이터를 빠르게 처리하면서도 비용 부담을 줄일 수 있게 됬다.

Clustrering은 대용량 데이터를 분산 처리하기 위한 기술로, 여러 서버를 결합하여 하나의 컴퓨터처럼 작동할 수 있도록 지원한다. 몇몇 서버에서 장애가 발생하더라도 나머지 서버에 작업을 자동으로 할당하여 작업을 지속할 수 있다.

분산처리를 지원하는 대표적인 open-source 소프트웨어는 두 가지가 존재한다.
+ Apache Hadoop(높은 처리 능력)
	+ 하나의 master 서버와 그 서버가 제어하는 여러 개의 slave 서버로 구성
	+ Master 서버는 전체 데이터 처리를 관리하고, slave 서버는 계산 작업을 처리
	+ Slave 서버의 수에 따라 처리 용량이 확장
+ Apache Spark(매우 빠른 반응 속도)
	+ 대규모 데이터를 메모리 내에서 병렬 분산 처리
	+ 메모리 내에서 실행되기 때문에 반복 처리 중 데이터를 디스크에 자주 읽고 쓰는 Hadoop보다 훨씬 빠르게 작동
	+ 모든 데이터를 메모리에 적재할 순 없기 때문에 TB 이상의 데이터를 처리하는 데는 적합하지 않음.

![500](Pasted%20image%2020241023143412.png)
### Database Technology
Cloud Provider는 대규모 데이터 분석 처리나 전자상거래와 같은 트랜잭션 처리를 포함해 사용자의 요구에 맞춘 다양한 데이터베이스 서비스를 제공. 주로 RDB, NoSQL 형식이 사용된다. 최근에는 빅데이터 분석 및 IoT 문제와 관련된 작업에서 NoSQL의 활용이 증가 중이다.
#### RDB
데이터를 행과 열로 구성된 표 형식으로 표현하며, 복잡한 데이터 관계를 처리하도록 설계되었다. RDBMS라는 전용 소프트웨어로 관리된다.
+ 데이터를 표 형식(행과 열)로 표현
+ SQL을 데이터 조작어로 사용
+ 엄격하게 데이터 일관성을 유지
+ 데이터 베이스 처리 용량을 향상시키기 위해 하드웨어를 확장해야 한다(scaling up)
#### NoSQL
전통적인 관계형 데이터 베이스와 다른 모든 데이터베이스를 말한다. 다양한 데이터 구조(데이터 저장 방식)을 가지지만, 공통적으로 대량의 데이터를 분산 처리할 수 있는 특성 덕분에 클라우드 서비스 구현에 적합하며, 주로 빅데이터 분석 작업에 사용된다.
+ Key-value: 모든 데이터가 인덱스된 값들로 구성. 구조가 단순하고, 확장성이 높으며 읽기 속도가 빠름
+ Column-Oriented: 데이터를 컬럼 단위로 저장하여 대량의 데이터를 빠르게 집계하고 업데이트할 수 있음. 쓰기 속도가 빠름
+ Document-Oriented: 복잡한 데이터를 문서로 저장하며, 데이터는 문서 단위로 저장. 복잡한 데이터를 처리하는 애플리케이션에 적합
+ Graph: 데이터 간의 관계를 그래프 구조로 형성. 그래프 기반의 빠른 검색을 지원
### Storage Technology
저장장치는 데이터를 저장하고 프로그램을 기록하는 장치이다. Cloud에서는 세 가지 방식으로 저장공간을 접근한다. 

![500](Pasted%20image%2020241023205837.png)
#### Block Storage
고정 크기로 나눠진 저장소의 논리적 볼륨을 블록 단위로 접근할 수 있는 유형. Fiber Channel(FC)와 iSCSI와 같은 전용 프로토콜을 사용해 저장장치에 접근한다. 서버와 저장공간 간의 데이터 전송 시 오버헤드가 적어 빠른 데이터 전송이 가능하다. 낮은 지연 시간이 요구되는 데이터베이스 애플리케이션에서 주로 사용한다.
#### File Storage
파일 단위로 데이터를 읽고 쓰는 유형. SMB와 CIFS 같은 파일 공유 프로토콜을 사용하여 파일 수준에서 데이터를 처리하며, 이는 주로 window OS에서 사용된다. 또한, UNIX와 Linux OS에서는 NFS를 사용해 구현한다. NAS도 이 유형으로 분류된다. 파일 서버에서 주로 사용되며, 파일 속성 관리와 접근 제어가 용이하다는 장점이 있다.
#### Object Storage
데이터를 객체 단위로 처리하며, 각 객체는 데이터와 관련 메타데이터로 구성되며 고유한 ID(URI)를 가진다. OS나 파일 시스템에 의존하지 않고 데이터를 저장하고 객체에 접근할 수 있다. HTTP 프로토콜을 통한 REST 기반 API를 사용하여 이 유형의 저장공간에 접근할 수 있다. 용량 확장이 용이하고, 저장할 수 있는 데이터 크기나 데이터 객체 수에 제한이 없다. 업데이트 빈도가 낮거나 장기 보관이 필요한 데이터를 저장하는 데 주로 사용된다.
### SDN(Software Defined Networking)
서버 가상화와 cloud가 빠르게 발전함에 따라 시스템 통합 관리와 운영 자동화가 진전되고 있다. 하지만 실제로 네트워크는 여전히 전통적인 방식으로 하드웨어 단위로 운영 및 관리되고 있다. 서버 가상화와 cloud는 네트워크 트랙픽의 급증과 네트워크 경로 변화를 야기하고, 이러한 변화에 대응하기 위한 네트워크 확장 및 수정, 자동화에 큰 어려움이 생기기 시작하였다.

위 문제를 해결하고 유연한 네트워크 변경을 실현하기 위해 네트워크 가상화를 가능하게 하고, 네트워크 구성 및 기능을 소프트웨어로 프로그래밍할 수 있게 하는 SDN이 해결책으로 등장하였다.
#### Basic of SDN
기존 네트워크 장치가 가지고 있던 통신의 전달 기능과 제어 기능을 분리하는 개념.
+ 제어 기능을 컨트롤러에 논리적으로 중앙 집중화하고, 
+ 데이터를 전달하는 방식을 소프트웨어로 정의.

서버 가상화와 유사하게 물리적 네트워크를 추상화해 하나의 물리적 네트워크 상에 여러 개의 가상 네트워크를 생성할 수 있게 한다. SDN이 등장하면서 스위치와 라우터 같은 네트워크 장치의 구조는 OS, 미들웨어, 애플리케이션이 통합된 수직 통합형 구조에서 기능이 개별 계층으로 분리되고, 오픈 인터페이스를 통해 연결되는 구로 변화하였다.
#### Impact of SDN Adoption
네트워크 장치는 컨트롤러에 의해 중앙에서 제어되기 시작하였다. 기업 사용자는 네트워크 운영 및 상태에 따라 소프트웨어를 통해 데이터 전송 경로를 유연하게 변경할 수 있게 되었다. 또한, 가상 서버에서 실행 중인 OS나 소프트웨어를 정지하지 않고 서로 다른 데이터 센터의 물리적 컴퓨터로 이동할 수 있는 live migration 기능을 적용하여 네트워크 구성을 유연하게 변경할 수 있었다. (기존 cloud만으로는 어려운 작업) 이 결과 데이터 센터 간의 자원 활용이 효과적으로 이루어질 수 있게 되었다.
### Edge Computing
서버를 cloud에 두지 않고, 스마트폰과 같은 단말 장치에 가까운 지역에 분산 배치하는 컴퓨팅 모델. 이러한 서버들은 단말 기기가 전송한 데이터를 처리한다. 

Cloud computing은 서버를 중앙집중화하여 처리하는 반면, edge computing은 단말 지점에 가까운 곳에 분산 처리하게 된다. IoT 애플리케이션처럼 모든 데이터를 cloud에서 집약적으로 처리하는 모델이 적합하지 않은 경우가 존재한다. 즉, 실시간 데이터 처리와 높은 신뢰성이 요구되는 상황에서는 edge computing을 통해 cloud 서비스 중단이나 네트워크 지연 문제를 회피할 수 있는 방법을 제공할 수도 있다.

![500](Pasted%20image%2020241023211836.png)
### GPU
ML 및 DL을 적용하는 분야가 확대되면서, NVIDIA와 같은 반도체 제조업체가 개발한 GPU에 대한 관심이 높아지고 있다. GPU는 원래 빠른 이미지 처리를 위해 개발되었으나, 최근에는 ML 및 DL과 같은 범용 컴퓨팅에도 널리 활용되고 있다.

CPU는 일반적으로 몇 개에서 수십개의 코어를 가지고 있으며, 복잡한 명령을 순차적으로 처리하는 데 뛰어나지만, GPU는 수백에서 수천 개의 코어가 있어 대량의 명령을 병렬로 처리하는 데 매우 효율적인 장치이다. 이 때문에 대량의 데이터를 빠르게 처리해야 하는 ML 및 DL와 같은 분야에서는 GPU 기반의 계산이 매우 효과적이다.
![500](Pasted%20image%2020241023212328.png)
### Data Center
Cloud service를 지원하는 서버와 네트워크 장비는 일반적으로 데이터 센터에 설치되며, 안전하게 운영되고 있다. 데이터 센터의 구조와 설계는 데이터를 효과적으로 관리하는 데 중요한 요소이다. 일반적으로 자연 재해의 위험이 적은 지역에서 견고한 지반 위에 건설된다. 대부분의 데이터 센터는 대규모 지진에도 견딜 수 있도록 내진 및 구조 강화 설계가 되어있다. 막대한 전력 소비량을 가지기 때문에 에너지 절약 대책이 필요하다.
### Microservice Architecture
Cloud Provider가 제공하는 서비스 중 Microservice Architeture라는 모델이 인기를 얻고 있다. 애플리케이션을 작은 서비스들로 구성해 각 서비스가 API와 같은 간단한 수단을 통해 상호 작용하는 방식이다.

Cloud Provider가 제공하는 서버, 저장공간, 데이터베이스, 네트워킹과 같은 서비스는 독립적으로 운영되며, 여러 개의 독립적인 구성 요소로 이루어져 있다. 각 구성 요소들은 느슨한 결합으로 작동하여 전체 기능을 구현하며, 이를 통해 cloud provider는 새로운 서비스를 신속하게 개발하고, 필요한 경우 새로운 구성 요소를 추가하거나 교체할 수 있다.
### Serverless Architecture
기업 사용자와 개발자는 Microservice로 구성된 cloud service의 각 구성 요소를 API로 결합하고 통합하여 독립적인 애플리케이션 개발, 서비스 생성, 시스템 구축이 가능하다. 

Cloud service가 환경 설정, 보안 패치, 백업 등을 포함한 전체 관리를 제공하는 경우, 사용자는 서버의 존재를 인식하지 않고 애플리케이션을 실행할 수 있다.  이러한 경우를 Serverless Architecture라고 하며 이 설계를 채택한 cloud service를 FaaS(Function as a Service)라고 한다.

![500](Pasted%20image%2020241023213102.png)

![500](Pasted%20image%2020241023213116.png)
