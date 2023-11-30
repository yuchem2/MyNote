space 보안은 access token을 기반으로 구성.  space와 통신하기 위해 먼저 appliaction은 access token을 얻고, 이 토큰을 통해 요청을 전송할 수 있음

예시
```javascript
GET https://mycompany.jetbrains.space/api/http/absences 
	Authorization: Bearer <here-goes-access-token> 
	Accept: application/json
```

access token은 space에서 application 인증뿐만 아니라 시스템에서 수행할 수 있는 작업을 정의한다. 

## Authorization subject
[[Space HTTP API]]를 호출할 때 application은 자체적으로 혹은 space가 user를 대신해 작동 가능
+ 자체적으로 작동하는 경우 space administrator가 application에 부여한 권한에 의해 제한된다. 
+ space가 user를 대신해 작동하는 경우 user가 appliaction에 부여한 권한에 따라 제한되며 사용자 자신이 가지고 있는 권한만 부여 가능

즉, 인증 대상은 appliaction의 권한과 user 권한으로 나뉘어 구별된다
## Get an access token
access token을 받기 위해 appliaction은 space에서 지원하는 인증 방법 중 하나를 사용해야 함

|                       Method                       | Best for                                                                |    On behalf of     | Details                                                                                                                                                                                                                                                                                                |
|:--------------------------------------------------:| ----------------------------------------------------------------------- |:-------------------:| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
|                  Permanent token                   | 권장 x, 개발 중인 application을 test하는 용도로 사용 가능               | user or application | 만료되지 않아 보안성이 떨어지지만 구현은 쉬움                                                                                                                                                                                                                                                          |
|         OAuth 2.0. Authorization Code flow         | 서버에 인증 logic이 있는 web application                                |        user         | application은 필요한 리소스의 범위도 포함하는 링크를 통해 user를 space에게 보내고, space는 이를 받아 지정된 redirect URI를 이용해 redirect. 이 전송에 access token 존재. 이때 refresh code flow으로 인해 `refresh_token`도 포함. `refresh_token`를 통해 자동으로 새로운 access token을 받을 수 도록 지원 |
|         OAuth 2.0. Client Credentials flow         | chatbots과 같이 대신하여 리소스에 접근하는 application                  |     application     | application은 `client_id`와 `client_secret`을 전송함으로써 access token을 받음. user 권한이 필요한 리소스는 이를 통해 접근 불가능.                                                                                                                                                                      |
|              OAuth 2.0. Implicit flow              | browser에 인증 logic이 있는 rich client web application                 |        user         | 더 이상 사용 x                                                                                                                                                                                                                                                                                         |
| OAuth 2.0 Resource Owner Password Credentials flow | 권장 x, 잠재적으로 user를 대신하여 리소스에 접근하는 script에 사용 가능 |        user         | user는 application에 space user credentials을 제공하고, application은 이를 이용해 user를 대신해 space에 대한 전체 접근 권한을 가짐. 안전하지 않기 때문에 사용하지 않는 것을 권장. 이 흐름은 등록된 모든 application에 기본적으로 활성화되어 있어 명시적으로 활성화할 필요 x                            |

## OAuth 2.0 endpoints 

| Endpoint       | URL                               |
| -------------- | --------------------------------- |
| Authentication | `<Space service URL>/oauth/auth`  |
| Token          | `<Space service URL>/oauth/token` |
## How to implement authorization 
[[Space SDK]] 위에 애플리케이션을 생성하는 경우 `SpaceHttpClient` 클래스와 해당 확장 method를 사용하여 인증 method를 구현 가능

[[Space SDK]]를 사용하지 않는 경우 일반 지침을 따라 구현 가능
### Permanent Token
특정 권한 범위를 가진 새 토큰을 생성한 뒤 원하는 곳에서 인증을 사용하면 됩니다. `Authorization`요청 헤더의 파라미터인 `Bearer`를 parmanent token으로 사용한다.

Permanent token은 본질적으로 OAuth 2.0 인증에 사용되는 임시 액세스 토큰보다 보안 수준이 낮다. Application, User 두 가지 유형으로 사용 가능. 
#### Application Permanent Token
1. Extensions | Applications에서 필요한 application을 연다
2. Permanent Tokens 탭을 열고 New permanent token을 클릭
3. 토큰 이름과 만료 날짜를 지정
4. Create를 클릭하고, 생성된 토큰을 안전한 위치에 복사. 토큰에 다시 접근할 수 없으므로 주의
5. 토큰을 생성한 후 Update하거나 Revoke 가능
#### Personal Permanent Token
1. 왼쪽 상단에서 아바타를 클릭한 다음 Preferences $\rightarrow$ Personal Tokens
2. New personal token 클릭
3. 새 토큰에 고유한 이름 지정
4. 토큰의 접근 범위 지정
	+ Full access
	+ Limited access $\rightarrow$ global, some project or chat channel
5. Create 클릭. 
6. 새 토큰이 포함된 대화 상자 창이 표시되면 토큰을 복사하여 안전한 위치에 저장. 
7. 복사한 후 대화 상자가 닫히고 토큰이 표시. 언제든지 Update & Revoke 가능

#### Implement
##### Kotlin Space SDK
```Kotlin
// URL of your Space instance 
const val spaceUrl = "https://mycompany.jetbrains.space" 
// Personal token 
val token = System.getenv("JB_SPACE_TOKEN") 
// Create a client 
val spaceClient = SpaceClient(token, spaceUrl) 

suspend fun getAllAbsences(): Batch<AbsenceRecord> { 
	// Use the client to make a request 
	return spaceClient.absences.getAllAbsences()
}
```
##### HTTP request
###### Request:
```js
GET https://mycompany.jetbrains.space/api/http/team-directory/locations?query=&type=Region&withArchived=true&$fields=id,archived,channelId,name,type,tz 
Authorization: BearereyJhbGciOiJSUzUxMiJ9.eyJzdWIiOiJhSGZ2eDEyZTU1dCIsImF1ZCI6ImNpcmNsZXQtd2ViLXVpIiwib3JnRG9tYWluIjoibXljb21wYW55Iiwic2NvcGUiOiIqKiIsIm5hbWUiOiJ0cmF2aXMud2lja2V0dCIsImlzcyI6Imh0dHBzOlwvXC9qZXRicmFpbnMuc3BhY2UiLCJwcmluY2lwYWxfdHlwZSI6IlVTRVIiLCJleHAiOjE1OTAxNTk2ODYsImlhdCI6MTU5MDE1OTA4Niwic2lkIjoiMXRjbU1CMkxGZzl1In0.VJaqfkGt2RCArKg9l6oZWpA5_29DrKXLYdEAQpKaP4TuA3kHmqn7xv90NabF6Inot8zfnK1pRUc07zSunxe1lCOK81N7_GeNgw6rHB_3S-XGoOAO-7OSVVH-duffpueUj-sWcBHfCI9iTofuTZgXUZ7IcJ_FP8vyNBhM_kgx-As 
Accept: application/json
```
###### Response: 
```json
200 
accept-ranges: bytes 
content-encoding: gzip 
content-security-policy: frame-ancestors 'none' 
content-type: application/json 
date: Fri, 22 May 2020 14:47:06 GMT 
referrer-policy: no-referrer 
status: 200 
vary: Origin 
x-frame-options: DENY 
[ 
	{ 
		"id": "2w9S8K2x3Aqy", 
		"name": "The Netherlands", 
		"tz": null, 
		"type": "Region", 
		"channelId": null, 
		"archived": false 
	}, 
	{ 
		"id": "1sjSCi2B6qdM", 
		"name": "Russian Federation", 
		"tz": null, 
		"type": "Region", 
		"channelId": null, 
		"archived": false 
	}, 
	{ 
		"id": "4Vsy4f3sCNkX", 
		"name": "USA", 
		"tz": null, 
		"type": "Region", 
		"channelId": null, 
		"archived": false 
	}, 
	{ 
		"id": "14fTZH1pMwJ5", 
		"name": "Germany", 
		"tz": null, 
		"type": "Region", 
		"channelId": null, 
		"archived": false 
	} 
]
```
### Authorization Code Flow
#### Basics
+ Space user를 대신한 승인
+ Client가 Server 측에서 실행되거나 완전히 Client brower(static web page)에서 실행되는 web application에 적합
```ad-caution
title: static web page에 대한 중요한 보안 참고 사항
color: 244, 92, 74

application이 space에 대한 authorization code flow을 수행하는 정적 리소스인 경우 Client Credentials Flow를 비활성화해야 한다. 정적 리소스인 경우 space가 명시적으로 application의 client ID와 secret을 client browser에 보내 문제가 생김. Client Credentials flow가 활성화되면 잠재적인 공격자가 이 데이터를 사용해 space의 application 설정을 수정할 수 있음. 

JetBrains Marketplace에 application을 추가하거나 설치 링크를 생성할 때 Client Credentials flow를 비활성화 할 수 있음
```

+ 리소스 소유자는 user-side rendering된 HTML user interface를 통해 appliaction에 접근 가능. 
+ application 자격 증명과 application에 발급된 모든 access token은 web server(혹은 정적 리소스의 경우 web browser)에 저장되고, 리소스 소유자에 노출되지 않고, 접근 불가능.
+ application은 필요한 리소스의 범위도 포함하는 링크를 통해 user를 space에게 보냄. user가 space에 로그인하면 space는 지정된 redircet URI를 사용하여 user를 다시 application으로 redirect. redircet에는 authorization code가 포함되어 있음. application은 authorization code를 사용해 access token을 얻음
+ space에서 authorization code flow을 통해 얻은 token은 제한된 기간 동안만 유효. 만료된 후 application은 refresh token flow를 사용해 새로 refresh해야함
+ [[Space SDK]]를 사용하는 경우 `Space` class의 helper methods를 이용해 흐름 구현 가능
```
     +----------+
     | Resource |
     |   Owner  |
     |          |
     +----------+
          ^
          |
         (B)
     +----|-----+          Client Identifier      +---------------+
     |         -+----(A)-- & Redirection URI ---->|               |
     |  User-   |                                 | Authorization |
     |  Agent  -+----(B)-- User authenticates --->|     Server    |
     |          |                                 |               |
     |         -+----(C)-- Authorization Code ---<|               |
     +-|----|---+                                 +---------------+
       |    |                                         ^      v
      (A)  (C)                                        |      |
       |    |                                         |      |
       ^    v                                         |      |
     +---------+                                      |      |
     |         |>---(D)-- Authorization Code ---------'      |
     |  Client |          & Redirection URI                  |
     |         |                                             |
     |         |<---(E)----- Access Token -------------------'
     +---------+       (w/ Optional Refresh Token)

   Note: The lines illustrating steps (A), (B), and (C) are broken into
   two parts as they pass through the user-agent.
```
#### PKCE
Space의 Authorization code flow는 PKCE 확장을 지원
PKCE(Proof Key for Code Exchange)는 authorization flow을 더욱 향상시키는 추가 보호 코드. desktop 및 mobile client application에 보다 안전하고 강력한 로그인 환경을 제공하기 위해 만들어졌으며 다른 OAuth 2.0 모범 사례 중에서 IETF에서 권장

PKCE의 작동 
1. Client(Application)는 "code verifier"라는 임의의 문자열을 생성
2. Client는 [[Hash Function]]을 사용하는 code verifier의 hash인 "code challenge"를 생성
3. Client는 authorization 요청에서 space에 code challenge를 보냄
4. Space는 code challenge를 저장하고 이 요청에 대해 생성한 authorization code와 연결
5. User가 authorization 요청을 승인하면 client는 authorization code를 받습니다. 그런 다음 client는 space에서 access token을 요청. authorization 코드와 함께 client는 original code verifier를 전송
6. Space는 수신된 code verifier에서 code challenge를 다시 생성. 그런 다음 새로 생성된 code challenge를 이전에 저장한 code challenge와 비교. 일치하면 space는 client에 access token을 발급
#### Implement
##### Kotlin Space SDK
```ad-tip
Authorization code flow가 활성화되면 space는 자동으로 refresh token flow를 활성화. access token은 600초 동안에만 유효하고, 이 시간이 지난 후에는 새로운 token은 발행해야 함.
```
아래 예시는 PKCE와 함께 Authorization Code Flow를 구현하는 방법. 작동하게 하려면, authentication tab의 application setting에서 Authorization Code Flow와 Require PKCE를 사용하도록 설정해야 한다. 

또한, Cilent ID와 Cilent secret이 application에게 제공되어야 한다. 만약 secret을 제공하지 않고, Authorization Code Flow를 활성화하려면, Allow public (untrusted) clients를 활성화해야 한다. 

```kotlin
package com.example 
import io.ktor.http.* 
import io.ktor.server.application.* 
import io.ktor.server.engine.* 
import io.ktor.server.html.* 
import io.ktor.server.netty.*
import io.ktor.server.response.* 
import io.ktor.server.routing.* 
import kotlinx.html.* 
import space.jetbrains.api.runtime.* 
import space.jetbrains.api.runtime.resources.teamDirectory 
import space.jetbrains.api.runtime.types.GlobalPermissionContextIdentifier 
import space.jetbrains.api.runtime.types.PermissionIdentifier 
import space.jetbrains.api.runtime.types.ProfileIdentifier 
import java.util.* 
import java.util.concurrent.ConcurrentHashMap 
// store these in your database, securely and persistently. 
val codeVerifiersByAuthProcessId: MutableMap<UUID, String> = ConcurrentHashMap() 

// base Ktor client 
val ktorClient = ktorClientForSpace() 

const val spaceUrl = "https://mycompany.jetbrains.space" 
const val appUrl = "https://myapp.url" 
const val showUsernameRoute = "/show-username" 
const val authorizeInSpaceRoute = "/authorize-in-space" 
const val redirectUri = "$appUrl$showUsernameRoute" 

// class that describes your app 
val spaceAppInstance = SpaceAppInstance( 
	// 'clientId' and 'clientSecret' are generated when you 
	// register the application(https://www.jetbrains.com/help/space/single-org-applications.html#specify-authentication-options) in Space 
	// Do not store id and secret in plain text 
	clientId = System.getenv("JB_SPACE_CLIENT_ID"), 
	clientSecret = System.getenv("JB_SPACE_CLIENT_SECRET"), 
	spaceServerUrl = spaceUrl, 
) 

fun Application.module() { 
	routing { 
		// Index page 
		get("/") { 
			call.respondHtml { 
				body { 
					button { 
						onClick = "window.location.href = '$authorizeInSpaceRoute'" +"Authorize in Space and show your username" 
					} 
				} 
			} 
		} 
			
		// When the user presses the button, they are redirected to this page. 
		// There, an ID of the auth process is generated, and a code verifier for this auth process is stored. 
		// Then the user is redirected to Space for the authentication and authorization. 
		get(authorizeInSpaceRoute) { 
			val authProcessId = UUID.randomUUID() 
			val codeVerifier = Space.generateCodeVerifier() 
			codeVerifiersByAuthProcessId[authProcessId] = codeVerifier 
			
			call.respondRedirect( 
				Space.authCodeSpaceUrl( 
					appInstance = spaceAppInstance, 
					// Specify the scope of resources that the application needs to access. 
					// In our case, we need to access user profiles. 
					// Learn more about permissions(https://www.jetbrains.com/help/space/request-permissions.html) 
					scope = PermissionScope.build( 
						PermissionScopeElement( 
							context = GlobalPermissionContextIdentifier, 
							permission = PermissionIdentifier.ViewMemberProfiles 
						) 
					), 
					redirectUri = redirectUri, 
					/** [OAuthAccessType.OFFLINE] for offline access on behalf of user */
					accessType = OAuthAccessType.ONLINE, 
					state = authProcessId.toString(), 
					codeVerifier = codeVerifier, 
				) 
			) 
		} 
			
		// After the user is authenticated in Space, they are redirected back to our app at this page. 
		// With this redirect, we receive the auth code, which we can exchange for an auth token. 
		// We retrieve the stored code verifier and send it to Space, so that we can be sure that the user is the same
		// person that started the auth process. 
		get(showUsernameRoute) { 
			suspend fun respondAuthError() { 
				call.respondHtml(HttpStatusCode.Unauthorized) { 
					head { 
						title("Authentication error") 
					} 
					body {
						p { 
							+"Authentication error." 
						} 
						button { 
							onClick = "window.location.href = '$authorizeInSpaceRoute'" +"Try again"
						} 
					} 
				} 
			} 
				 
			val authProcessIdString = call.parameters["state"] ?: run { 
				respondAuthError() 
				return@get 
			} 
				 
			val authProcessId = try { 
				UUID.fromString(authProcessIdString) 
			} catch (_: IllegalArgumentException) { 
				respondAuthError() 
				return@get 
			} 
				 
			val authCode = call.parameters["code"] ?: run { 
				respondAuthError() 
				return@get 
			} 
				 
			val spaceAccessTokenInfo = try { 
				Space.exchangeAuthCodeForToken( 
					ktorClient = ktorClient, 
					appInstance = spaceAppInstance, 
					authCode = authCode, 
					redirectUri = redirectUri, 
					codeVerifier = codeVerifiersByAuthProcessId[authProcessId] ?: run { 
						respondAuthError() 
						return@get 
					}, 
				) 
			} catch (_: PermissionDeniedException) { 
				respondAuthError() 
				return@get 
			} 
			
			/** If you requested [OAuthAccessType.OFFLINE] access, you can use [SpaceTokenInfo.refreshToken] 
			* with [SpaceAuth.RefreshToken]. */ 
			val spaceClient = SpaceClient(ktorClient, spaceAppInstance, SpaceAuth.Token(spaceAccessTokenInfo.accessToken))
			val profile = spaceClient.teamDirectory.profiles.getProfile(ProfileIdentifier.Me) 
			val username = profile.username 
				
			val msg = "Hello ${username}!" 
				
			call.respondHtml { 
				head { 
					title(msg) 
				} 
				body { 
					p { 
						+msg 
					} 
					button { 
						onClick = "window.location.href = '/'" +"To home page" 
					} 
				} 
			} 
		} 
	} 
} 


fun main() { 
	embeddedServer(Netty, port = 8080, module = Application::module).start(wait = true) 
}
```
##### Generic instructions
###### Initial request
인증 프로세스를 시작하기 위해서 appliaction은 user's browser로 authentication endpoint `<Space service URL>/oauth/auth`를 redirect: 
```js
${Space Service URL}/oauth/auth?response_type=code&state=${State}&redirect_uri=${Client redirect URI}&request_credentials=${Request credentials mode}&client_id=${Client ID}&scope=${Scope}&access_type={online|offline}&code_challenge={code_challenge}&code_challenge_method={code_challenge_method}
```
이 요청에서 
+ `code_challenge`는 PKCE 확장을 사용할 때 필요
+ `code_challenge_method`는 선택 사항. 기본은 "plain"

example: 
```js
https://mycompany.jetbrains.space/oauth/auth?response_type=code&state=9b8fdea0-fc3a-410c-9577-5dee1ae028da&redirect_uri=https%3A%2F%2Fmyservice.company.com%2Fauthorized&request_credentials=skip&client_id=98071167-004c-4ddf-ba37-5d4599fdf319&scope=0-0-0-0-0%2098071167-004c-4ddf-ba37-5d4599fdf319&code_challenge=E9Melhoa2OwvFrEMTJguCHaoeK1t8URWbuGJSstw-cM&code_challenge_method=S256
```
위 요청에서 매개변수를 추가하면 `access_type=offline` 인증 코드가 access token과 함께 refresh token도 반환. 
```js
https://mycompany.jetbrains.space/oauth/auth?response_type=code&state=${State}&redirect_uri=${Client redirect URI}&request_credentials=${Request credentials mode}&client_id=${Client ID}&scope=${Scope}&access_type=offline&code_challenge={code_challenge}&code_challenge_method={code_challenge_method}
```
space에서 토큰을 얻으려면 application이 authorization request에 있는 매개변수에 대한 값을 제공해야 한다. <a href="https://www.jetbrains.com/help/space/authorization-code.html#parameters">참고</a>
###### Handle response(authorization code)
리소스 소유자가 access 권한을 부여하면 space는 authorization code를 발행하고 `appliaction/x-www-form-url-urlencoded`를 사용하여 redirectection URI의 query 구성 요소에 다음 매개변수를 추가하여 appliaction에 전달

| Parameter | Description                                                                                                                                                                                             |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| code      | space에서 생성한 authorization code. 유출 위험을 방지하기 위해 발급된 직후 만료. 두 번 이상 사용되는 경우 space는 요청을 거절. authorization code는 application identifier와 redirection URL에 바인딩됨 | 
| state     | space는 이 값을 매개변수로 `redirectUri`에 추가. application은 이 값을 사용해 authorization process를 식별하고 계속.                                                                                    |

example:
```js
HTTP/1.1 302 Found 
Location: https://myservice.company.com/authorized?code=SplxlOBeZQQYbYS6WxSbIA&state=xyz
```
application은 인식할 수 없는 response parameter를 무시.
###### Handle error response
리소스 소유자는 missing or invalid redircetion URI 이외의 이유로 요청이 실패한 경우 space는 application에게 `application/x-www-form-urlencoded`를 사용하여 redirectection URI의 query 구성요소에 다음 매개변수를 추가하여 application에게 알림
```ad-note
title: error
color: 190, 198, 207
A single ASCII [USASCII] error code
+ `invalid_request`: 요청에 필수 매개변수가 누락되었거나, 잘못된 매개변수 값이 포함되어 있거나, 매개변수가 두 번 이상 포함되어 있거나 형식이 잘못됨
+ `unauthorized_client`: application은 이 방법을 사용하여 authorization code를 요청할 권한이 없음
+ `access_denied`: 리소스 소유자 또는 space가 요청을 거부
+ `unsupported_response_type`: space는 이 방법을 사용한 authorization code 획득을 지원하지 않음
+ `invalid_scope`: 요청한 범위가 유효하지 않거나 알 수 없거나 형식이 잘못됨
```

```ad-note
title: error_description
color: 190, 198, 207
application developer가 무엇이 잘못되었는지 이해하는데 도움이 되는 추가적인 정보를 제공하는 Human-readable ASCII [USASCII] text
```
###### Exchange authorization code for a token
application은 authorization code를 수신한 후 이를 access token으로 교환할 수 있음. 초기 요청에서 `access_type=offline`이면 space는 refresh token도 같이 리턴

appolication은 HTTP 요청 entity-body에서 UTF-8로 인코딩된 `application/x-www-form-urlencoded` format를 사용해 다음과 같은 매개변수를 보냄으로써 space token endpoint에 요청합니다.  
```js
POST /oauth/token 
Host: ${Space Service URL} 
Accept: application/json 
Authorization: Basic ${base64(${Client ID} + ":" + ${Client secret})} 
Content-Type: application/x-www-form-urlencoded 

grant_type=authorization_code&code=${Code received on a previous step}&redirect_uri=${Client redirect URI}&code_verifier={code_verifier}
```
이 요청에서 
+ `code_verifier`는 PCKE 확장을 사용할 때 필요
+ `client_id`는 Public Clients(신뢰할 수 없는 서비스)용 PKCE 확장을 사용하는 경우 및 Auth Header가 생략되는 경우 필요합니다.
```ad-tip
title: PKCE 확장을 사용하는 경우
+ Confidential Clients(신뢰할 수 있는 서비스)에는 `Auth Header`가 필요
+ Public Clients(신뢰할 수 없는 서비스)의 경우 `Auth Header`를 생략할 수 있지만, 이 경우 `client_id`가 필수
```

example: <a href="https://www.jetbrains.com/help/space/authorization-code.html#parameters">매개변수 참고</a>
```js
POST /oauth/token 
Host: Space.company.com 
Accept: application/json 
Authorization: Basic OTgwNzExNjctMDA0Yy00ZGRmLWJhMzctNWQ0NTk5ZmRmMzE5OmVBVXlLZ1ZmaFNiVg0K 
Content-Type: application/x-www-form-urlencoded 

grant_type=authorization_code&code=SplxlOBeZQQYbYS6WxSbIA&redirect_uri=https%3A%2F%2Fmyservice.company.com%2Fauthorized&code_verifier=dBjftJeZ4CVP-mB92K27uhbUJU1p1r_wW1gFWFOEjXk
```
###### Handle token response
토큰 요청이 유효하고 승인된 경우 space는 access token을 발급하고, 요청하는 경우 refresh token을 발급. 요청이 실패했거나 유효하지 않은 경우 space server는 오류 응답을 반환

성공적이 응답 예시: 
```json
HTTP/1.1 200 OK 
Content-Type: application/json;charset=UTF-8 
{ 
	"access_token":"1443459450185.0-0-0-0-0.98071167-004c-4ddf-ba37-5d4599fdf319.0-0-0-0-0%3B1.MCwCFC%2FYWvLjHdzOdpLleDLITJn4Mz9rAhRklCoZ2dlMkh2aCd1K5QQ89ibsxg%3D%3D", 
	"expires_in":600, 
	"refresh_token": "mF36POk6yJV_adQssw5c_RJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c" 
}
```
###### Refresh an access token
application은 UTF-8로 인코딩된 `application/x-www-form-urlencoded` format을 사용해 HTTP 요청 entity-body에 매개변수를 추가해 token endpoint `<Space service ULR>/oauth/token`에 refresh 요청을 만듦.
+ `grant_type`: OAuth 2.0 요청에 grant tpye을 지정. 필수적 요소. `refresh_token`의 값을 설정
+ `refresh_token`: 필수적 요소. application에 발급된 refresh token
+ `scope`: 필수적 요소. space의 특정 리소스에 접근하는 데 필요한 권한 목록. (공백으로 구분)

refresh token은 추가 access token을 요청하는 데 사용되는 오랫동안 지속되는 자격 증명이므로, 발급된 application에 바인딩되어 관리. application 유형이 confidential이거나 application에 application 자격 증명이 발급되었거나 다른 인증 요구 사항이 할당된 경우 space서버로 인증해야 한다. <a href="https://datatracker.ietf.org/doc/html/rfc6749#section-3.2.1">RFC 6749</a>

application은 transport-layer security를 사용해 다음 형식으로 HTTP 요청을 만듦.
```js
POST /oauth/token 
Host: ${Space Service URL} 
Authorization: Basic ${base64(${Client ID} + ":" + ${Client secret})} 
Content-Type: application/x-www-form-urlencoded 

grant_type=refresh_token&refresh_token=${Refresh token received from Space}
```

example:
```js
POST /oauth/token 
Host: mycompany.jetbrains.space 
Authorization: Basic czZCaGRSa3F0MzpnWDFmQmF0M2JW 
Content-Type: application/x-www-form-urlencoded 

grant_type=refresh_token&refresh_token=tGzv3JOkF0XG5Qx2TlKWIA
```

space will:
1. Confidential appliaction 또는 application 자격 증명(또는 기타 인증 요구사항)이 발급된 appliaction에 대해 인증을 요구
2. appliaction 인증이 포함된 경우 application을 인증하고, refresh token이 인증된 application에 발급되었는지 확인
3. refresh token을 검증

refresh token이 유효하고 승인된 경우 space는 access token을 발급

새로운 access token과 함께 space는 refresh token을 발행할 수 있으며 이 경우 application은 이전 refresh toeken을 삭제하고 새 토큰으로 교체해야 함. space는 application에 새 refersh token을 발급한 후 이전 refresh token을 삭제할 수 있음. 새로운 refresh token일 발급되는 경우 토큰의 범위는 application이 요청에 포함하는 refresh token의 범위와 동일해야 한다.
###### Refresh token flow errors
요청이 확인에 실패했거나 유효하지 않은 경우 space는 오류 응답을 반환

```ad-note
title: error
color: 190, 198, 207
A single ASCII [USASCII] error code
+ `invalid_request`: 요청에 필수 매개변수가 누락되었거나, 지원되지 않는 매개변수 값이 포함되어 있거나, 매개변수가 반복되거나, 여러 자격 증명이 포함되어 있거나, application 인증을 위해 두 개 이상의 메커니즘을 사용하거나, 형식이 잘못되었습니다. 
+ `invalid_client`: application 인증에 실패. space는 HTTP 401 상태 코드를 반환하여 지원되는 HTTP 인증 체계를 나타낼 수 있음. application이 "Authorization" 요청 header field를 통해 인증을 시도한 경우 space 서버는 HTTP 401 상태 코드로 응답하고 application에서 사용하는 인증 체계와 일치하는 "WWW-Authenticate" 응답 header field를 포함
+ `invalid_grant`: 제공된 인증 부여 또는 refresh token이 유효하지 않거나, 만료되었거나, 취소되었거나, 인증 요청에 사용도니 redirection URI과 일치하지 않거나, 다른 application에 발급된 상태
+ `unauthorized_client`: 인증된 application은 이 권한 부여 유형을 사용할 권한이 없음. 
+ `unsupported_grant_type`: 해당 권한 부여 유형은 space에서 지원하지 않음
+ `invalid_scope`: 요청한 범위가 유효하지 않거나 알 수 없거나 형식이 잘못되었거나 리소스 소유자가 부여한 범위 초과
```

```ad-note
title: error_description
color: 190, 198, 207
application developer가 무엇이 잘못되었는지 이해하는데 도움이 되는 추가적인 정보를 제공하는 Human-readable ASCII [USASCII] text
```

```ad-note
title: error_uri
color: 190, 198, 207
이 URI는 Human-readable web page를 error에 대한 정보와 함께 식별한다. application 개발자에게 error에 대한 추가적인 정보를 제공
```

위 매개변수 들은 "application/json" media type을 사용하는 HTTP 응답의 entity-body에 포함되어 있음. JSON 데이터 형태를 띈다.

example: 
```js
HTTP/1.1 400 Bad Request
Content-Type: application/json;charset=UTF-8
Cache-control: no-store
Pragma: no-cache

{
	"error":"invalid_request"
}
```
### Client Credentials Flow
