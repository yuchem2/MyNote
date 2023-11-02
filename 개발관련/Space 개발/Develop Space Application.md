## 개요
space marketplace에 배포할 application을 개발하는 방법을 정리 (by. Beyond-Imagination 팀장)
<a href="https://jetbrains.com/help/space/applications.html">https://jetbrains.com/help/space/applications.html</a>

---
## 구조
+ application
	+ 웹페이지 구현
	+ application에 필요한 api server
	+ application 사용을 위한 FE
+ <a href="https://www.jetbrains.com/help/space/add-webhooks.html">space webhook</a>
	+ space에서 발생하는 event를 application에서 받기 위한 방법
	+ application에서 원하는 event를 선택하여 특정 url로 전달받을 수 있음
+ <a href="https://jetbrains.com/help/space/api.html">space api</a>
	+ space에 특정 기능을 호출하기 위한 api
	+ 이슈 생성, 메시지 전송 등 다양한 기능을 제공

## 분류
+ Single-org applications
	+ space에 직접 등록해서 사용
	+ 필요한 설정을 직접 해야 함
		+ webhook
		+ endpoint
+ Multi-org applications
	+ marketplace를 통해 배포
	+ 실제 배포용
---
## 인증
space application 구현 시 인증에 대한 구현이 필요. 
<a href="https://www.jetbrains.com/help/space/authentication-and-authorization.html">https://www.jetbrains.com/help/space/authentication-and-authorization.html</a>
### 인증 방식
api 요청 시 acess token을 사용하여 인증
```javascript
GET https://mycompany.jetbrains.space/api/http/absences
	Authorization: Bearer <here-goe-acess-token>
	Accept: applications/json
```
### 인증 대상
acess token 종류에 따라 space application이 두 가지 형태를 띈다
1. 유저 권한
2. application 권한

이를 위해 application에서 필요한 권한에 따라 인증 방식도 달라지게 된다.
<a href="https://www.jetbrains.com/help/space/authentication-and-authorization.html#authorization-methods">https://www.jetbrains.com/help/space/authentication-and-authorization.html#authorization-methods</a>

```
example 
	유저 권한으로 메시지 전송 시 채널의 메시지 전송자가 유저로 표시
	application 권한으로 메시지 전송 시 채널의 메시지 전송자가 application

	메시지 전송 시 사용한 access token에 따라 전송자가 바뀐다. 이외 다른 모든 api도 동일
```
### 권한
api 별로 필요한 권한이 다르며 이 권한을 가진 유저/application의 acess token을 사용해야지만 해당 api를 실행할 수 있다. 
+ 유저 acess token: 유저와 동일한 권한
+ application acess token: 사전에 권한 신청/승인 과정이 필요
#### 권한 신청
+ <a href="https://www.jetbrains.com/help/space/request-permissions.html#8dccdcb4">api를 통한 권한 신청</a>
	+ 실제 prod application일 때 해당 방식으로 권한을 받음
	+ api로 요청을 하면 관리자/앱 설치자/채널 관리자 등이 허락을 해야 한다
+ <a href="https://www.jetbrains.com/help/space/request-permissions.html#request-permissions-manually">application page에서 권한 설정</a>
	+ 개발 중 or 단일 organization application에서 사용하는 방식
	+ app 관리자가 직접 space application page에서 권한을 부여
---
## 요청
space에서 application으로 보내는 요청으로, 다음과 같은 상황에서 발생할 수 있다.
+ webhook
+ application 채널
+ application button
### 검증
요청을 송신한 측이 실제 space가 맞는지 확인하는 과정이 필요. 여러가지 방법이 있지만 space측에서 `public key` 방식을 추천한다. <a href="https://www.jetbrains.com/help/space/verify-space-in-application.html#verifying-requests-using-a-public-key">https://www.jetbrains.com/help/space/verify-space-in-application.html#verifying-requests-using-a-public-key</a>
### 데이터
space는 json 형식으로 데이터를 전송. 이 데이터는 3가지로 구분된다.
+ Payload class(className)
	+ data에 type에 대한 내용
	+ 모든 요청에 필수로 존재함
	+ 이 값을 읽어 어떤 요청인지 구분해야 한다
	+ <a href="https://www.jetbrains.com/help/space/process-payload-from-space.html#payload-classes">여러 type이 존재</a>
+ Class-specific payload data
	+ class에 따라 존재하는 데이터
	+ className을 읽고 어떤 데이터가 존재할지 알 수 있음
+ Common payload data
	+ 모든 요청에 공통으로 존재하는 데이터
```json
// example
{
	// Payload class
	"className": "MessagePayload",
	// Class-specific payload data
	"message": {
		"messageId": "CsT000CsT",
		"channelId": "3FhQeS2URbeY",
		"messageData": null,
		"body": {
			"className": "ChatMessage.Text",
			"text": "help"
		},
		"attachments": null,
		"externalId": null,
		"createdTime": "2021-05-21T17:01:33.7672"
	},
	// Common payload data
	"accessToken": "",
	"verificationToken": "abc1234",
	"userId": "1mEGCd1FvoAh",
	"serverUrl": "https://mycompany.jetbrains.space",
	"clientId": "2ulA3W2Vltg6",
	"orgId": "8fd4d79a-d164-4a71-839a-ff8f8bcd6beb"
}
```

---
## webhook
space에서 발생한 event의 notification을 application 서버에서 받는 방법
### 등록 방법
2가지 등록 방법이 존재하나 배포 버전에서는 무조건 api를 사용해야 한다
+ <a href="https://beyond-imagination.jetbrains.space/extensions/httpApiPlayground?resource=applications_xxx_webhooks&endpoin=rest_create">api 사용</a>
	+ 배포 버전
	+ multi-org
+ application 페이지에서 수동 등록
	+ 개발 버전
	+ single-org

### webhook payload
+ `className`: WebhookRequestPayload로 전달
+ `webhookId`: webhook의 id
+ `payload`: webhook이 전달하는 payload
+ `payload.className`: <a href="https://www.jetbrains.com/help/space/http-api-reference.html#WebhookEvent">event의 class name</a>
```json
{
    "className": "WebhookRequestPayload",
    "accessToken": "",
    "verificationToken": null,
    "serverUrl": "https://mycompany.jetbrains.space",
    "clientId": "a1b83dac-6e26-46cb-b3cb-7e264eab5e9e",
    "orgId": "o0Ghr0EewX8",
    // id issued to a webhook during registration
    "webhookId": "3EZUCW3RmsZY",
    "payload": {
        // event class name
        "className": "ProfileOrganizationEvent",
        // event-specific data
        "meta": {
            "principal": {
                "name": "admin",
                "details": {
                    "className": "CUserPrincipalDetails",
                    "user": {
                        "id": "2ZJrHC3ouUQz"
                    }
                }
            },
            "timestamp": {
                "iso": "2021-06-15T14:51:35.545Z",
                "timestamp": 1623768695545
            },
            "method": "Created"
        },
        "member": {
            "id": "4Zg1mS4aBsOm"
        },
        "joinedOrganization": true,
        "leftOrganization": false
    }
}
```
---
## Application UI
사용할 UI에 대해 api를 통해 등록하는 과정이 필요하다
<a href="https://beyond-imagination.jetbrains.space/extensions/httpApiPlayground?resource=applications&endpoint=http_patch_ui-extensions">https://beyond-imagination.jetbrains.space/extensions/httpApiPlayground?resource=applications&endpoint=http_patch_ui-extensions</a>
### Homepage
application은 각자만의 homepage를 갖는다. 해당 homepage는 space에서 application을 진입했을 때 첫 페이지로 보여지게 된다. 
<a href="https://www.jetbrains.com/help/space/application-homepage.html">https://www.jetbrains.com/help/space/application-homepage.html</a>
### Custom menu
space 내부 서비스에 메뉴 버튼을 추가할 수 있다. 메뉴를 추가할 수 있는 서비스 종류는 다음과 같다. 
+ Chat message
+ Code review
+ Document
+ Issue
+ Meeting
<a href="https://www.jetbrains.com/help/space/custom-menu-items.html#prepare-the-homepage-content">https://www.jetbrains.com/help/space/custom-menu-items.html#prepare-the-homepage-content</a>
### Chatting
space channel에서 사용하는 chatbot을 추가할 수 있다. chatbot에 command를 추가할 수 있으며 command는 chat UI를 제공한다
<a href="https://www.jetbrains.com/help/space/receive-messages.html">https://www.jetbrains.com/help/space/receive-messages.html</a>

---
## Application 배포
개발한 application은 jetbrains marketplace에 등록하여 space에 배포할 수 있다
<a href="https://www.jetbrains.com/help/space/multi-org-applications.html#distribute-the-application-via-jetbrains-marketplace">https://www.jetbrains.com/help/space/multi-org-applications.html#distribute-the-application-via-jetbrains-marketplace</a>
### multi-org appliaction 배포
필수 check list
1. init payload 처리
2. UI extensions setiing
3. permission 요청
4. webhook 설정
5. 기타 필요 작업(application db 설정 등)

<a href="https://www.jetbrains.com/help/space/distribute-your-application.html">https://www.jetbrains.com/help/space/distribute-your-application.html</a>