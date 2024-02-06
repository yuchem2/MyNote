## /signUp
### 설명
+ 회원가입을 합니다.
### request
+ Headers
```
no headers
```
+ body
```json
{
	id: string,
	password: string,
	nickname: string,
}
```
## /login
### 설명
+ 등록된 정보로 로그인을 시도합니다
### request
+ Headers
```
no headers
```
+ body
```json
{
	id: string,
	password: string,
}
```