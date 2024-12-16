## AWS Introduction
AWS는 Cloud computing service를 시장에 제일 빠르게 도입하였습니다. 현재 100개가 넘는 서비스를 제공하고 있으며, 매년 새로운 서비스를 추가하고 있습니다. 매년 이런 정보는 "AWS re:Invent" 행사를 통해 공개되고 있습니다.

| Year | Description                                               |
| ---- | --------------------------------------------------------- |
| 2003 | The idea originated from Chris Pinkham and Benjamin Black |
| 2006 | Started AWS in the form of a platform (launch of EC2, S3) |
| 2010 | Migrated to AWS for Amazon.com                            |
| 2012 | Held the first AWS re:Invent conference                   |
| 2014 | Amazon Aurora                                             |
| 2015 | AWS IoT and Amazon Machine Learning                       |
| 2016 | Amazon Lex, Polly, and Rekognition                        |

다음과 같은 여러 서비스들이 존재합니다.
+ Computing
+ Storage
+ Database
+ Migration
+ Networking & Content Delivery
+ Developer Tools
+ Media Services
+ Machine Learning
+ Analytics
+ Security, Identify & Compliance
+ Mobile Services
+ AR & VR
+ Application Integration
+ AWS Cost Management
+ Customer Engagement

### AWS Free Tier
AWS Free Tier는 AWS 서비스를 특정 제한에서 12개월동안 체험해볼 수 있는 서비스를 말한다. 새로 계정을 만들 때 Free Tier를 선택하면 된다.

### AWS Pricing Check
각 서비스마다 고유의 비용 정책이 존재하고, 얼마나 비용이 부과되는 지 확인할 수 있다. 특정 서비스를 사용하기 전에, 먼저 비용에 대한 정보를 알아보고 자신의 예산에서 서비스를 선택하는 것이 좋다.

### AWS Global Infra
+ Region
	+ 클라우드 기반 서비스를 제공하는 물리적인 위치
	+ 사용자가 데이터를 물리적으로 위치할 지리적 위치를 선택할 수 있도록 허용한다.
	+ 지역을 선택함으로써 각 국가의 규제 요건을 충족할 수 있다.
+ Available Zone(AZ)
	+ 하나의 Region은 여러 개의 AZ로 구성된다.
	+ 각 AZ는 하나 이상의 데이터 센터로 구성되어 있다.
	+ 각 데이터 센터는 네트워크로 상호 연결되어 있다.

Region은 각 계정이 생성된 Region에 따라 기본적으로 그 Region에 할당된다. 이후 필요에 따라 다른 Region으로 변경이 가능하다. Region간 데이터 복제는 자동으로 이루어지지 않으며, 일반적으로 추가 비용이 발생한다. 그러므로 Region을 선택할 때 신중한 고려가 필요하다. 일부 서비스는 특정 Region에만 제공될 수도 있다.
#### Edge Location
CloudFront 서비스에서 콘텐츠 전달을 위한 endpoint 역할을 수행한다. CloudFront는 Amazon에서 제공하는 보안 콘텐츠 전달 서비스로, S3와 결합하면 자주 액세스되는 미디어 파일을 소비자에 더 가까운 위치에 캐싱하게 된다. 현재 전세계 50개 이상의 edge location이 존재한다.

+ 동작 예시
	+ 사용자가 보려는 비디오 파일이 아시아 태평양(도쿄) region의 S3 bucket에 저장되어 있다. 사용자는 해당 비디오 파일의 다운로드 URL을 알고 있다. 이 경우 사용자의 위치와 상관없이 매번 비디오 파일을 다운로드할 때 인터넷을 통해 도쿄 데이터 센터에 연결된다.
	+ CloudFront틀 통해 배포할 시 사용자는 원래 URL 대신 새 CloudFront URL을 제공받는다. 초기에는 사용자가 도쿄 데이터 센터에 연결되지만, CloudFront는 이후 접근을 위해 가까운 edge location에 콘텐츠를 캐싱한다. 이후부터는 가까운 edge location에 연결되어 콘텐츠를 제공 받는다.
## AWS ML Support Service
### AWS IAM: Amazon Identity & Access Management
특정 서비스에서 어떤 사람이 접근할 수 있는지 접근 제어를 하게 해주는 서비스로, 글로벌 서비스이다(Region에 특정한 서비스 x). **IAM은 AWS 리소스에 대해 "누가 접근할 수 있는지"와 "무엇을 할 수 있는지"를 제어할 수 있도록 하여 사용자와 리소스에 대한 접근 권한을 지정함으로써 보안을 강화한다.** 이러한 IAM의 기능은 다음과 같다.
+ Secure user mangement
+ Configure security credentials
+ Password expiration policy
+ MFA settings
+ Enable user control over AWS resources
#### Root Account
+ 결제 및 비밀번호 변경과 같은 작업을 수행한다. 
+ AWS 계정 및 모든 리소스에 대한 무제한 접근 권한이 제공된다.
+ Amazon은 루트 계정을 **일상적인 작업에 사용하지 않는 것**을 권장한다.
	+ 일상적인 작업을 위해 별도의 IAM 사용자를 생성해야 한다.
	+ 각 사용자에게 적절한 수준의 권한을 부여하기 위해 그룹과 정책을 설정할 수 있다.
	+ IAM 계정으로 로그인하여 AWS 서비스를 이용한다.
+ 루트 계정은 다른 사람과 공유하지 않아야 한다.
+ 루트 계정에 대한 다단계 인증(MFA) 설정을 강력히 권장하고 있다.
#### IAM Account
각 IAM 사용자는 조직 내의 개인 혹은 애플리케이션에 해당한다. 각 IAM 사용자 계정은 **고유한 로그인 주소, 비밀번호, 그리고 액세스 키**를 가진다. IAM 사용자 계정은 별도의 AWS 계정이 아니고, 루트 계정 내에서 존재하는 계정이다.

![500](Pasted%20image%2020241216164842.png)

고유한 로그인 주소, 비밀번호는 AWS 관리 콘솔에 접근하기 위함이고, 액세스 키는 AWS 서비스를 프로그래밍 방식으로 접근하기 위한 도구이다. 각 IAM 사용자 계정에 할당된 권한은 AWS 관리 콘솔에서 사용자가 접근할 수 있는 영역과 관련되어 있다. IAM 사용자가 반드시 실제 사람을 의미하지는 않는다. 

권리자 권한이 있는 IAM 사용자 계정을 먼저 만든 후 애플리케이션을 위한 별도의 IAM 사용자 계정을 만드는 것이 좋다.

#### Identity Federation
개인 또는 애플리케이션이 Active Directory, SAML 토큰, Facebook과 같은 방법으로 인증한 후 임시 IAM 사용자 계정을 통해 AWS 서비스에 프로그래밍 방식으로 접근할 수 있도록 하는 기능. 해당 기능의 절차는 다음과 같다.
1. 사용자는 먼저 외부 자격 증명 제공자에 로그인하고, 제공자로부터 토큰을 받는다.
2. 이 토큰을 IAM에 제출한다.
3. 토큰을 제출하면, AWS에서는 리소스에 접근할 수 있는 임시 자격 증명을 사용자에게 제공한다.

![500](Pasted%20image%2020241216165306.png)

+ Company identity Federation
	+ AWS 외부의 회사 저장소에 저장된다.
	+ 네트워크를 통해 이미 인증된 사용자는 새 자격 증명으로 AWS에 접근할 필요 없이 바로 사용할 수 있다.
+ Web identity Federation
	+ Facebook, Google, Amazon과 같은 잘 알려진 다른 조직에 저장된다.
	+ 사용자가 이미 가지고 있는 Facebook, Amazon, Google 계정으로 로그인할 수 있도록 허용하는 웹 혹은 모바일 앱을 개발하는 경우 이상적이다.
#### IAM Group
IAM 사용자들로 구성된 개념으로, 동일한 정책을 그룹의 모든 구성원에게 적용할 수 있다. IAM group은 조직 내의 각 사용자마다 개별적으로 권한을 설정하는 대신, 권한을 간단하게 관리할 수 있는 방법을 제공한다. IAM 사용자를 IAM group에 할당하고, group 수준에서 권한을 관리하게 된다. 대규모 조직에서는 부서나 직무 기능에 따라 그룹을 설정하는 것이 일반적이다.

![500](Pasted%20image%2020241216165825.png)
#### IAM Policy
사용자, 그룹, 역할에 대한 권한을 부여하는 JSON 문서이다. IAM 정책 문서는 수행할 작업 및 리소스 목록을 포함한다. 사용자를 관리할 때 그룹을 활용하는 경우 정책은 사용자와 그룹에 모두 적용될 수 있다. 이 경우 사용자의 권한은 사용자에게 직접 적용된 정책과 그룹 구성원으로서 상속받은 정책의 조합에 의해 결정된다. 
+ 사용자 기반 정책: 사용자에게 적용된 정책
+ 리소스 기반 정책: 리소스에게 적용된 정책
### Amazon S3
Amazon에서 제공하는 간단한 저장공간 서비스. 객체 데이터를 클라우드에서 안전하게 저장하고 검색할 수 있다. EC2와 함께 가장 널리 사용되는 서비스 중 하나이다. S3에 저장된 데이터는 region 내의 여러 AZ와 서버에 자동으로 분산된다. 객체 기반 저장 서비스로, 파일 데이터를 저장하는 데 이상적이다. 

운영체제를 설치하거나 EC2 인스턴스의 저장 공간(서버로 사용)은 불가능하다. Amazon S3에 저장된 데이터는 key-value 형식으로 저장된다. 저장할 수 있는 데이터의 양에는 제한이 없지만, 단일 파일의 크기는 5TB를 초과할 수 없다.

Amazon S3에서 사용되는 용어는 다음과 같다.
+ bucket: S3에서 파일을 저장하는 폴더와 같은 역할 수행. 
	+ 버킷 이름은 전 세계적으로 고유해야 하며, 동일한 이름으로 버킷을 생성할 수 없다. 
	+ 생성된 버킷은 접근 가능한 사용자로 설정할 수 있다.
	+ 생성된 버킷에 대해 가능한 작업 권한을 설정할 수 있다.
	+ 버킷에 저장된 각 객체는 객체 키와 메타데이터를 가진다.
+ object key: S3 버킷에서 객체를 구별하는 UTF-8 문자열.
	+ 객체 키는 파일이 처음 버킷에 업로드할 때 할당된다.
	+ 기본적으로 키 이름은 업로드된 파일의 이름이다.
	+ 내부적으로 데이터를 알파벳 순으로 저장한다. 그러므로 이름이 비슷한 파일들은 실제 물리 디스크에서 비슷한 위치에 저장된다.
	+ 파일을 저장할 때 파일 이름을 순차적으로 지정할지 혹은 동일한 접두어로 저장할 지를 미리 결정하는 것이 좋다.
+ object value: 저장한 실제 데이터로, 단일 데이터는 최대 5TB 길이로 구성될 수 있다.
+ version ID: 객체의 버전을 식별하는 문자열 값.
	+ 객체를 버킷에 업로드할 때 version ID를 할당한다. 
	+ 객체 버전 관리를 활성화하면 업데이트마다 새로운 version ID가 생성된다.
	+ object key와 version ID는 객체를 고유하게 식별할 수 있게 해준다.
+ storage class: 객체를 읽는 빈도에 따라 다양한 클래스를 가진다.( Standard, Standard-IA, RRS, Glacier)
	+ 객체가 어떻게 저장할 지를 결정하고, 저장 방법에 따라 데이터를 읽을 때 추가 비용이 발생할 수 있다.

 Amazon S3는 다음과 같은 요인에 따라 비용이 부과된다.
 + 저장 비용, 요청 비용, 저장 관리 가격, 데이터 전송 가격, 전송 가속화 가격
### AWS Cognito
앱을 사용하는 유저들의 인증(authentication)을 만들고, 해당 앱 계정에 로그인 할 수 있도록 하는 서비스.

일반적으로 웹/모바일 앱을 만들 때 로그인 혹은 결제 기능을 구현하는 것은 필수적이지만, 구현이 어려운 부분이다. 
+ 인증(Authentication): 사용자가 누구인지 확인하는 과정. 일반적으로 로그인할 때 제공한 정보로 사용자 확인.
+ 권한부여 authorization): 인증된 사용자가 특정 자원이나 서비스를 사용할 수 있는지 확인하는 과정. 

이러한 기능들을 편리하게 사용할 수 있도록 제공하는 서비스로, IDaaS(IDentity as a Service)라고 불리는 서비스이다. (ex) amazon cognito, auth0 by Okta, Firebase 등)

![500](Pasted%20image%2020241216171552.png)

Cognito는 OAuth(Authentication protocol)과 OIDC(Authentication + Authorization protocol)을 활용해 다음과 같은 기능을 제공하고 있다.
+ 사용자 데이터 베이스 관리
+ 외부 웹/앱 애플리케이션 인증 제공
+ 외부 API 혹은 AWS 리소스에 대한 인증 제공 

![500](Pasted%20image%2020241216171841.png)

![500](Pasted%20image%2020241216171855.png)
### AWS DynamoDB
높은 성능의 확장 가능한 cloud-based NoSQL DB를 제공하는 서비스. 기능은 다음과 같다.
+ AWS에서 DB table 생성, 읽기/쓰기 작업 허용
+ SSD 저장소를 사용해 고속 데이터 처리 가능
+ 데이터를 여러 서버에 분산시켜 빠르고 일관된 액세스 시간을 보장 (규모에 관계 없이 10ms 이하의 응답 시간 보장)
+ 사용자가 선택한 region의 모든 AZ에 데이터를 복재
### AWS Lambda
인프라 구성 없이도 코드를 실행시킬 수 있는 서비스. 즉, 서버를 직접 설정하고 관리할 필요 없이 cloud infra에서 코드를 실행시킬 수 있게 해주는 서비스 = serverless service. 다음과 같은 예시에서 사용할 수 있다.
+ File processing
+ Stream processing
+ Web Applciation: lambda와 다른 AWS 서비스를 결합하면 강력한 웹 앱을 구축할 수 있다. 자동으로 확장되고 여러 데이터 센터에 걸쳐 높은 가용성 구성을 통해 실행된다.
+ IoT Backends: lambda를 사용해 웹, 모바일, IoT 외부 API 요청을 처리하는 서버리스 백엔드 구축 가능
+ Mobile Backends: lambda와 amazon API gateway를 사용해 api 요청을 인증하고 처리하는 백엔드 구축 가능. AWS amplify를 사용해 iOs, android, web, react native 프런트엔드를 쉽게 통합할 수 있다.

![500](Pasted%20image%2020241216172446.png)

Python, Node.js, Ruby, Java, C#, GO로 함수를 작성할 수 있다. lambda에 사용되는 용어들은 다음과 같다.
+ functions: lambda에서 코드를 실행하기 위해 호출할 수 있는 리소스. 함수에 전달된 event 혹은 다른 AWS 서비스에서 함수로 전송한 event를 처리하는 코드를 포함한다. 
+ trigger: lambda 함수를 호출하는 리소스 또는 구성. 
	+ 함수를 호출하도록 구성할 수 있는 AWS 서비스와 이벤트 소스 매핑을 포함
	+ 이벤트 소스 매핑은 stream이나 queue에서 항목을 읽고 함수를 호출하는 lambda 리소스
+ event: lambda 함수가 처리할 데이터를 포함하는 JSON 형식의 문서
	+ runtime은 event를 객체로 변환하여 함수 코드에 전달한다. 함수를 호출할 때 이벤트의 구조와 내용을 사용자가 결정한다.
+ handler: lambda에서 코드 실행을 요청하는 메서드. 핸들러가 호출되면 lambda는 함수와 데이터를 서버에 제출한다.
+ context: 실행 환경과 관련된 유용한 정보를 저장한다. (종료까지 남은 시간, 애플리케이션 이름 등)
## Machine Learning Application Services
AWS에는 ML에 대해 SaaS와 PaaS 형태의 서비스를 제공하고 있다.
+ SaaS
	+ Amazon Comprehend: text data를 인식하게 해주는 LLM 서비스
	+ Amazon Lex: 텍스트와 보이스에 대한 대화형 서비스(chatbot)을 만들 수 있는 서비스
	+ Amazon Rekogintion: DL 기반의 API 서비스로, 이미지와 동영상에서 물체를 인식하고, 식별할 수 있게 하는 서비스
+ PaaS
	+ Amazon SageMaker: Could 환경에서 ML 개발을 할 수 있도록 환경을 제공하는 서비스
	+ Amazon DeepLens: Cloud 환경에서 wireless 동영상 카메라와 관련된 개발 환경을 제공하는 서비스. CNN을 학습시키고, wireless 동영상 카메라에 배포할 수 있음
### Amazon Comprehend
별도의 프로비저닝이 필요 없는 웹 서비스. DL 기반 NLP와 주제 모델링을 위한 엔진을 제공한다. 텍스트 분석을 위한 사전 학습된 DL 모델을 제공한다.
+ 아마존 제품 리뷰 데이터를 기반으로 한 사전 학습 모델
+ 사전 학습된 모델은 우수한 성능을 보장하며, 사용자가 별도로 모델을 개발하고 학습하는 번거로움을 피할 수 있다.

![500](Pasted%20image%2020241216174331.png)

복잡한 ML 모델을 생성, 배포, 유지 관리하는 걱정 없이 비지니스 문제를 해결하기 위한 NLP 분석을 가능하게 한다. 사용되는 용어들은 다음과 같다.
+ entity: 단어 범주를 의미(상업 제품, 날짜, 이벤트, 위치, 조직, 사람, 수량, 제목, 기타)
+ key phrases: 텍스트에서 핵심 구문과 신뢰도 점수를 반환. 특정 주제에 대해 사람들이 이야기하고자 하는 정보를 식별하는 데 유용
+ sentiment: 텍스트에서 전체적인 기분을 나타냅니다. 고객 코멘트나 제품 리뷰 분석 시 유용
+ syntax: 명사, 대명사, 형용사, 동사 등 다양한 구문 요소를 식별. 구문 요소를 식별하는데 사용
+ topic modeling: 기사에서 가장 잘 설명하는 주제를 결정하는 방법
+ supported languages: 독일어, 영어, 스페인어, 이탈리아어, 포르투갈어, 프랑스어, 일본어, 한국어, 힌디어, 아랍어, 중국어
### Amazon Lex
음성 및 텍스트를 모두 지원하는 대화형 인터페이스를 제공하는 웹 서비스. Amazon Alexa의 대화형 엔진을 사용해 애플리케이션에 챗봇을 통합할 수 있도록 한다.
+ mobile application: AWS Mobile SDK를 사용해 챗봇을 만들 수 있다.
+ web application: AWS JavaScript SDK를 사용해 챗봇을 만들 수 있다.

Amazon Lex에서 사용되는 용어들은 다음과 같다.
+ chatbot: 자연어로 입력을 받아 인간의 대화를 시뮬레이션하는 프로그램
	+ 일반적으로 인터넷을 통해 접근할 수 있으며, 다양한 상업적 서비스에 사용된다.
	+ NLP 기술을 활용해 사용자의 질문을 이해하고 적합한 응답을 결정할 수 있다.
+ client application: Lex는 음성 및 텍스트를 모두 지원한다. 그러나 프론트엔드 인터페이스를 포함하고 있지 않는다.
	+ 텍스트 기반 챗봇의 경우, 사용자가 내용을 입력할 수 있는 채팅 창이 프론트엔드 인터페이스에 해당한다. 
	+ API를 활용해 자체 데스크탑 혹은 웹/앱 애플리케이션을 구축할 수 있다. 
	+ 메시징 플랫폼을 프론트엔드 인터페이스로 사용할 수도 있다.
+ intent: 챗봇 사용자가 취할 수 있는 행동을 나타내는 객체. intent를 생성하려면 다음과 같은 정보가 필요하다.
	+ 그 목적을 효과적으로 전달하는 설명적인 이름
	+ 의도 기능을 실행하기 위한 예시 구문
	+ 추가적인 사용자 입력 세부사항. 예를 들어, 신발 주문을 위한 intent는 신발 크기, 색상, 재질 등과 같은 정보가 필요할 수 있다.
+ slot: 의도의 매개변수로, 사용자가 제공해야 하는 값을 나타낸다. Lex의 ML 모델을 훈련하는 데 사용되는 값의 목록이다. 다음과 같은 유형이 존재한다.
	+ Built-in slot types: 이메일 주소, 날짜, 시간, 위치 등과 같은 일반적인 입력을 처리할 수 있는 amazon에서 제공하는 slot
	+ Custon slot types: 사용자가 직접 정의하는 열거형 값으로, Lex는 ML 기법을 사용해 사용자가 다양한 값을 입력하더라도 효과적으로 작동할 수 있도록 합니다. 
+ utterance: intent와 관련된 구문을 말한다. 사용자가 챗봇에 말을 하거나 입력을 하면 Lex는 제공된 발화를 기반으로 해당 의도를 식별하고 처리한다.
	+ intent를 생성할 때 utterance 목록을 지정해야 하며, Lex는 이 발화들을 사용해 ML 모델을 훈련시킨다. 훈련된 모델은 챗봇이 유사한 구문을 효과적으로 처리하는 데 도움을 준다.
	+ ML 모델을 사용함으로써 챗봇은 개발자가 챗봇을 생성할 때 정의한 정확한 단어에만 의존하지 않는다. 대신, 사용자가 다른 입력 값을 사용할 때도 변형된 구문을 이해할 수 있는 능력 덕분에 잘 작동한다.
+ programming model: Lex는 다음과 같은 API 유형을 제공한다.
	+ 모델 구축 API: AWS cloud에서 챗봇 앱을 구축하고 배포하는 데 사용
	+ 런타임 API: 분산된 챗봇 앱을 운영하는 데 사용
	+ Lex는 웹 기반 관리 콘솔을 제공하며, 이 프로그램은 챗봇을 구축, 배포 및 테스트할 수 있는 기능을 포함하고 있습니다. 이 콘솔은 사용자가 Lex로 만든 챗봇을 테스트할 수 있는 기능도 제공
### Amazon SageMaker
Amazon SageMaker는 AWS 클라우드 인프라에서 Python 코드를 사용하여 데이터 탐색, 특성 엔지니어링, 머신 러닝 모델 훈련 기능을 제공하는 온라인 서비스입니다. 
+ programming model
	+ Amazon SageMaker Python SDK: ML 작업을 수행할 수 있는 Python SDK
	+ AWS boto3 SDK: SageMaker가 다양한 AWS 서비스와 상호작용할 수 있도록 하는 Python SDK
	+ Programming language SDK: Ruby, Java 등 다양한 프로그래밍 SDK 지원
	+ AWS CLI: Amazon SageMaker와 상호작용할 수 있는 CLI 제공
+  Notebook Instance
	+ Jupyter Notebook이 미리 구성된 EC2 인스턴스를 실행 가능
	+ SageMaker 관리 콘솔을 통해 Jupyter Notebook 서버에 접근 가능
	+ Numpy, Pandas, Matplotlib, 데이터 탐색, 특성 공학, 일반적인 데이터 분석, 모델 훈련/배포/검증 가능
### Amazon Rekogntion
이미지 및 비디오 분석을 위한 DL 모델을 제공하는 웹 서비스입니다. 객체 감지, 객체 위치 추적, 장면 분석, 활동 감지, 얼굴 인식 등 서비스 제공.
+ AWS web에서 직접 이미지를 입력해 label detection 가능
+ S3 + lambda + DynamoDB를 이용해 S3에 새로운 데이터가 입력될 때마다 이미지를 분석해 그 결과를 DynamoDB에 저장할 수 있음