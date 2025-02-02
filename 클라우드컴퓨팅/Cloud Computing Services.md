### Types of Cloud Computing
Cloud Computing은 제공하는 서비스의 형태에 따라 IaaS, PaaS, SaaS로 구분될 수 있다. 

![500](Pasted%20image%2020241022143033.png)

![500](Pasted%20image%2020241022215225.png)
#### IaaS(Infrastructure as a Service)
가장 초기 단계의 Cloud computing service로, CPU, 하드웨어와 같은 컴퓨팅 자원(Computing resoure)을 네트워크를 통해 제공하는 서비스이다. 특정 IaaS 서비스는 가상 서버와 온라인 저장장치를 포함한다. 

가상 서버는 cloud provider에 의해 물리적인 서버의 CPU, 메모리, 저장장치 등 하드웨어 자원을 소프트웨어로 나눈 것이다. 물리적인 서버를 구매하는 것 대신에, 사업자들은 많은 가상 서버를 필요한 만큼 필요할 때 만들 수 있다. 

가상 서버는 매우 빠르게 생성할 수 있다. 한번 생성되고 나면, 자유롭게 필요한 만큼 규모를 축소하거나 증가시킬 수 있다. **가상 서버는 OS에 설치된 데이터베이스, 미들웨어 및 애플리케이션과 같은 소프트웨어를 자유롭게 실행할 수 있지만, 사용자가 이를 직접 설치하고 관리해야 한다.**

대부분의 cloud provider는 IaaS를 사용한 만큼 지불하는 방식(pay-as-you-go) 혹은 기본 월정액요금 방식을 책정하고 있다. 또한, 데이터 업로드 및 다운로드 시 전송된 데이터 양에 따라 요금이 부과될 수 있다.(무료인 경우도 존재)
##### 예시
IaaS 사용 사례로는 웹사이트 서버가 있을 수 있다. 웹사이트를 운영하면서 프로모션 이벤트를 위한 웹 페이지를 새로 개설할 경우 짧은 시간에 많은 트래픽이 몰리는 경우가 생길 수 있다. 이러한 경우에 해당 기간에 대량의 컴퓨팅 자원을 일시적으로 빌려 사용하고, 기간이 끝나면 자원을 줄이는 방식으로 유연하게 자원량을 변경함으로써 구축 비용과 운영 비용을 절감할 수 있다.

또한, IaaS는 기업 사용자용 근태 관리 시스템과 같은 주요 시스템 기반에도 도입되고 있다. 예를 들어 기업용 통합 패키지인 ERP(Enterprise Resoure planning) 시스템도 이제 IaaS 상에서 운영되곤 한다.

대표적인 IaaS 서비스로는 AWS에서 제공하는 Amazon Elastic Compute Cloud(EC2)가 있다.
##### 장단점
+ 장점
	+ 가장 유연한 cloud service model
	+ 물리적인 서버가 없음
	+ 필요한 자원만큼 지불하면 됨
	+ 규모 증설 및 축소가 자유로움
	+ 저장장치, 서버, 네트워크, 처리 능력의 배포를 자동화
+ 단점
	+ 보안 위험 존재
	+ 인프라를 효율적으로 다루는 방법을 배워야 함
	+ 인터넷 연결에 의존
#### PaaS(Platform as a Service)
기업의 애플리케이션 실행, 개발 환경을 서비스로 제공하는 모델이다. 기업 사용자가 처음부터 자체 애플리케이션 개발 환경을 구축하는 것은 시간이 많이 소모되는 작업. 이러한 점에서 PaaS는 Java, PHP, Ruby와 같은 프로그래밍 언어를 지원하는 사전 구축된 애플리케이션 실행 환경과 데이터베이스를 제공한다. **인프라를 구축하고 유지할 필요 없이 그 기반을 사용할 수 있어, 애플리케이션을 빠르게 개발하고 서비스를 제공할 수 있다.**

PaaS와 IaaS의 차이는 서버, 네트워크, 보안 부분이 cloud provider에게 위임되어 구축 및 운영이 더 쉬워진다는 것. 
+ IaaS는 인터넷을 통해 가상화된 컴퓨팅 자원을 제공하는 데 중점을 둠
+ PaaS는 복잡한 설정 없이 애플리케이션을 빠르게 배포하고 실행할 수 있는 플랫폼을 제공
##### 예시
PaaS는 주로 개발 및 테스트를 위한 대량의 처리 능력이 필요할 때 혹은 자체 애플리케이션의 peak load를 분산할 때 사용된다. 또한 인터넷 접속이 필요한 모바일 서비스에도 적합하다. IoT에서 생성되는 센서 데이터와 같은 대량의 데이터를 효율적으로 수집하고 처리하는 플랫폼으로 주목 받고 있다.

대표적인 PaaS 서비스와 소프트웨어로는 Salesfore의 Force.com, Cyborg의 kintone, 오픈 소스 기반의 PaaS 소프트웨어인 Cloud Foundry, Microsoft의 Azure 등이 있다.
##### 장단점
+ 장점
	+ 매우 저렴하다
	+ 높은 확장성과 유연성을 가짐
	+ 애플리케이션을 쉽게 생성하고 배포 할 수 있음
	+ 협업이 쉬움
	+ 업데이트가 쉬움
+ 단점
	+ 보안 위험 존재
	+ 다른 클라우드로 이관이 어려움
	+ 최적화가 어려움
	+ 호환성 문제가 있을 수 있음
#### SaaS(Software as a Service)
주로 비지니스에서 사용되는 소프트웨어 기능을 네트워크를 통해 필요한 만큼 서비스로 제공하는 서비스. 여러 기업이 단일 서버를 공유하는 multi-tenant 서비스를 제공하지만, 데이터를 기업 사용자 별로 분리해 보안을 보장한다.

소프트웨어 업데이트는 기업 사용자가 아닌 cloud provider가 담당해 항상 최신 기능을 사용할 수 있고, 소프트웨어의 버그가 방치되지 않는다. 

서비스 계약을 체결하고, 사용자 계정이 준비되면 바로 서비스를 사용할 수 있다. 패키지 소프트웨어를 구매하는 것에 비해 도입까지의 시간이 크게 단축될 수 있다.

다른 프로그램들의 연산, 처리 등 논리적인 부분들이 사용자의 컴픁터에서 실행되는 반면, SaaS 애플리케이션은 연산, 처리 등의 논리 부분이 cloud에서 실행된다.
##### 예시
대표적인 SaaS 서비스는 CRM(Customer Relationship Management), e-mail, groupware, Google Apps 등이 있다.
##### 장단점
+ 장점
	+ 언제 어디서든, 어떤 장치로도 접속이 가능
	+ 업데이트나 설치가 필요하지 않음
	+ 확장성
	+ 비용 절감
+ 단점
	+ 강력한 접근 제어가 필요
	+ 제공자에 의해 종속되어 있음(Lock-in)
	+ 엄격한 규칙이 적용됨
### Cloud Computing Models
![500](Pasted%20image%2020241022215647.png)
#### Public Cloud
Cloud Provider가 시스템을 구축하고 네트워크를 통해 불특정 다수의 기업과 개인에게 서비스를 제공하는 경우를 말한다. 기업이나 개인의 방화벽 외부에 배포되어 사용되고, 사용자 조직이 자체 IT 자산을 소유하지 않고도 컴퓨팅 자원을 서비스로 사용할 수 있다. 필요한 컴퓨팅 자원을 짧은 시간 안에 저렴한 비용으로 얻을 수 있고, 운영 관리 부담이 줄어드는 장점이 존재.
+ 장점
	+ 비용 절감 가능
	+ 높은 가용성과 확장성
	+ 전문 관리 서비스 존재
	+ 사용이 쉽고 편리
+ 단점
	+ 보안 위험 문제
	+ 시스템 및 네트워크 제한 사항
	+ 비용 증가 가능성 존재
#### Private Cloud
Cloud Provider가 서비스 사용자 혹은 제공자의 데이터 센터에 클라우드 관련 기술을 사용해 전용 환경을 구축함으로써 컴퓨팅 자원을 유연하게 활용하는 형태이다. 가상화 및 자동화와 같은 클라우드 관련 기술의 사용은 시스템의 성능과 비용을 최적화해 유연한 맞춤화를 가능하도록 한다.
+ 장점
	+ 성능
	+ 보안
	+ 가용성
	+ 자원
	+ 제어
	+ 유연성
+ 단점
	+ 비용
	+ 유지 관리
	+ 배포
	+ 확장성
	+ 원격 접근
#### Hybrid Cloud
On-premise 시스템을 public 및 private 클라우드와 같은 클라우드 서비스와 결합해 사용하는 서비스를 말한다. 
![500](Pasted%20image%2020241022215936.png)
### Cloud vs. On-Premise
먼저 두 시스템의 비용적인 부분에서 비교하면 다음과 같다. 

|                    | On-premise                                                                       | Cloud                               |
| ------------------ | -------------------------------------------------------------------------------- | ----------------------------------- |
| Initial Costs      | Data center, servers, storage, network equipment 등의 초기 개발, 물자 비용 및 설치 소요시간 등이 필요 | 오직 사용한 자원에 대한 비용만 지불(유연한 사용)        |
| Operational Costs  | 시설 관리, 하드웨어 임대, 유지 관리, 회선 비용, 인건비 등 유지비용                                         | 일정한 월정액 요금이 필요로 하지만, 관리비가 없어 인건비 감소 |
| Uasage Flexibility | 사용량을 앞서 예측해야 하므로 과도한 지출이나 부족한 지출이 발생할 수 있음                                       | 오직 사용한 양에 대해서만 지불하면 됨               |
| Cost Trends        | 하드웨어와 유지비용이 증가하거나 안정적이거나 변동이 존재                                                  | 제공자 간의 경쟁으로 인해 가격이 하락하고 있음          |
| Management         | 직접 관리가 필요                                                                        | cloud provider에 의해 관리               |
표만 살펴보면 항상 cloud가 좋은 것처럼 보이지만, 서로 장단점이 존재한다. 특정 상황에서 장단점을 비교해 더 좋은 형태를 선택하는 것이 좋다. 여러가지 요소(시스템 사용, 수명, 잠재적인 보상 등)을 고려해 시스템을 결정해야 한다.
+ On-Premise가 더 좋은 경우
	+ On-Premise가 더 저렴한 경우: 시스템이 5년 이상 사용 중, 대규모 운영을 하는 경우, 현재 시스템이 안정적인 장기 성능을 보이는 경우
	+ Cloud로 전환(Migration)이 비용이 더 비싼 경우: 기존 on-premise 시스템의 수정, 대량 데이터 전송이 필요, 오랜 신뢰성을 가진 시스템을 이전하는 경우

구축 및 확장성을 통해 두 시스템을 비교하면 다음과 같다.
+ On-Premise
	+ 기업 맞춤형 설계
		+ 시스템이 개별 회사의 요구에 맞게 조정
		+ 데이터 센터, 회선, 네트워크 장비 등 구성 요소를 별도로 조달 가능
	+ 내부 관리
		+ 계획부터 기능 확장까지 전체 수명 주기를 직접 관리하며, 일관된 사용을 위한 가치 최적화에 집중할 수 있다.
		+ 모든 작업이 내부에서 진행되며, 숙력된 인력이 필요하고, 상당한 비용이 발생
+ Cloud
	+ 제공업체의 원할한 서비스
		+ 조달부터 구축까지 원스톱 서비스 제공
		+ 연중무휴 24시간 지원 및 유지 관리
	+ 유연한 자원 및 위험 감소
		+ 비즈니스 필요에 따라 가상 서버와 같은 자원을 쉽게 확장하거나 축소 가능
		+ 구축 및 운영 아웃소싱으로 시스템 소유의 위험을 줄이고, 인재가 핵심 비즈니스 작업에 집중하도록 할 수 있음
	+ 중소기업(SMEs)에 대한 혜택
		+ 중소기업이 대규모 투자 없이도 최첨단 IT 서비스에 접근할 수 있음
		+ 중소기업이 대기업과 유사한 규모로 경쟁할 수 있도록 지원
### Safety and Reliability of Cloud
Cloud Computing은 기업에 여러 가지 이점을 제공하지만, 사용되는 서비스와 관련된 안정성과 신뢰성 측면의 이해가 필요하다. 일반적으로 Cloud Service는 다음과 같은 리스크가 존재한다.
+ IaaS: 클라우드 제공업체의 하드웨어 고장으로 인한 데이터 손실이나 서비스 중단 가능성
+ Network: 통신 도청, 중간자 공격, Identity spoofing, 불충분한 네트워크 관리로 인한 downtime 등에 위험
+ 데이터를 cloud에 위임: 기업 사용자는 정보 처리를 제공업체에 위임해 보안 리스크에 대한 완전한 통제를 유지하는 데 잠재적인 어려움을 초래할 수 있음
+ Governance Challenges
	+ 클라우드 사용의 고유한 위험은 사고를 초래할 수 있는 사건 관리가 어렵게 만듦
	+ 제공업체의 파산이나 서비스 중단과 같은 예기치 않은 사건이 서비스 연속성을 위협할 수 있음

이러한 위험을 대처하기 위한 안정성과 신뢰성 대책은 다음과 같은 것들이 존재한다.
+ IaaS 관련 문제를 완화하기 위한 백업 가상 서버 활용
+ 기업에 대한 권장 사항
	+ 사용자 측과 제공업체 측의 보안 리스크를 지속적으로 평가
	+ 사용자와 제공업체 간의 책임을 명확히 이해
	+ 강력한 보안 조치를 구현하고 백업을 유지
### Responsibility of Using Cloud Service
사용자가 cloud 서비스를 이용하면서 서비스 책임은 다음과 같은 것들이 있을 수 있다.
+ 계약 전 명확성(Pre-contractual Clarity)
	+ 서비스 계약을 체결하기 전에 제공업체와 사용자 간의 책임 분담을 명확히 이해하는 것이 중요
+ 전형적인 규정(Typical Stipulations)
	+ 대부분의 계약서에는 제공업체가 사용자의 인터넷 접근이나 사용자의 환경(인터넷 설정, 애플리케이션, 데이터베이스 등)에서 발생하는 문제로 인한 중단에 대한 책임이 없음을 명시.
+ 사용자 주의 의무(User’s Due Diligence)
	+ 사용자는 서비스 사양을 철저히 검토하여 양측의 책임 범위를 파악해야 한다. 사용자의 책임에 해당하는 사항은 독립적으로 해결해야 한다.
+ 서비스 모델에 따른 변동성
	+ IaaS: 제공업체는 하드웨어, CPU, 저장장치 및 클라우드 소프트웨어에 대한 책임
	+ SaaS: 미들웨어에서 애플리케이션에 이르기까지 모든 것이 제공업체 책임
	+ Combined Models: PaaS와 IaaS와 같은 서비스가 동시에 사용될 때 책임 부담이 더 복잡해 질 수 있음
