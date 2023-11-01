> Last modified: 25 May 2023

Application is an external server-side service or a client-side program ([[JavaScript(JS)]], desktop, or mobile) that can interact with Space thorugh:
+ [[Space SDK]]
+ [[Space HTTP API]]
## Interaction between Space and Appliactions
space와 appliaction은 양방향으로 서로 통신이 가능하다
+ application가 space에게 데이터 전송
	+ chat channel을 통한 메시지 전송
	+ projects, team members와 같은 space entities 정보 요청
+ space가 application에게 데이터 전송
	+ webhook을 통해 events notification 전송
	+ user message를 전송

다음과 같은 방법으로 space user는 application과 상호작용 가능
+ application에게 chat message를 전송 (e.g. a chatbot)
+ application에게 받은 chat message의 버튼을 클릭
+ application에게 제공받은 링크를 클릭
+ 특정 space entitiy에 application으로부터 추가된 custom menu를 클릭
+ `iframe`을 통해 application user interface와 함께 상호작용
+ 정적 웹 페이지로부터 space와 상호작용
## Hosting of Applications
현재는 개발자가 만든 서버에서만 application을 host 가능. 이후 space에서 바로 host 가능한 형태 제공 예정
## Authentication and authorization
space에 대한 모든 요청은 모두 인증된 사용자로부터 보내져야 함. space에게 요청을 보내기 위해서는 applciation은 먼저 acess token을 얻어야 한다. 

가장 안전한 방법은 OAuth 2.0 인증이며 permanent token과 함께 application을 제공가능
## Distribution
배포 방법에 따라 두 가지 유형이 존재
### Single-org
특정 하나의 space organization에 연관된 appliaction. space에 수동으로 등록해야 하며, 인증 방법, application endpoint, 필수 권한 등 필요한 모든 데이터를 지정해야 한다.
### Multi-org
공개 배포용으로 여러 space organization에 설치할 수 있다. 설치 링크 or JetBrains Marketplace를 통해 설치할 수 있다. 

사용자가 application을 설치하면, space는 `InitPayload`를 aplliaction endpoint에 전송. 이를 수신하면 application은 space access 자격 증명을 사용해 새 space organization을 저장. 그 후 api 호출을 통해 space에서 자체적으로 구성. 

## [[Space HTTP API]]
application과 space가 통신하는 유일한 방법. 
<img src="https://resources.jetbrains.com/help/img/space/httpapiplayground.png">
## [[Space SDK]]
어떠한 언어로도 생성 가능하지만, [[Kotlin]] 이나 [[.NET]]에 경우 [[Space SDK]]를 이용해 개발 가능
