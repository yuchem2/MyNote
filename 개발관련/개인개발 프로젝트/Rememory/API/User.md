## [POST] /users/signup
### 설명
+ 회원가입을 합니다.
### request
+ Headers

|Name|Type|Required|Description|
|:-:|:-:|:-:|:-:|
|clientToken |string|O|fe client token |
+ body
```json
{
	provider: string,
	id: string,
	password: string,
	nickname: string,
}
```

|   Name   |  Type  | Required |                Description                |
|:--------:|:------:|:--------:|:-----------------------------------------:|
| provider | string |    O     | 회원가입 제공자 (rememory, google, kakao) |
|    id    | string |    O     |                 사용자 id                 |
| password | string |    O     |               사용자 passwd               |
| nickname         | string       | O         | 사용자 닉네임                                          |
### response
```
http 204 status code
```
## [POST] /users/login
### 설명
+ 등록된 정보로 로그인을 시도합니다
### request
+ Headers

|Name|Type|Required|Description|
|:-:|:-:|:-:|:-:|
|clientToken |string|O|fe client token |
+ body
```json
{
	id: string,
	password: string,
}
```
### response
#### 성공
```json
{
	jwt: string,
	nickname: string,
}
```

|  Name   |  Type  | Required |     Description     |
|:-------:|:------:|:--------:|:-------------------:|
| jwt | string |    O     | 발급받은 jwt |
| nickname        | boolean       | O         | 로그인한 사용자 닉네임                    |
## [POST] /users/logout
### 설명
+ 로그인된 계정을 로그아웃합니다
### request
+ Headers

| Name | Type | Required | Description |
| :--: | :--: | :--: | :--: |
| clientToken | string | O | fe client token |
| Authorization | string | O | login 시 발급받은 token |
### response
```
http 204 status code
```

## [GET] /users/check-id/:id
### 설명
+ id 중복검사
### request
+ Headers

|Name|Type|Required|Description|
|:-:|:-:|:-:|:-:|
|clientToken|string|O|fe client token|
+ url params

|Name|Type|Required|Description|
|:-:|:-:|:-:|:-:|
|id |string |O|중복 검사할 id |
+ body
```
no body
```
### response
```json
{
	success: boolean,
	message: string
}
```

|  Name  |  Type   | Required | Description  |
|:------:|:-------:|:--------:|:------------:|
| sucess | boolean |    O     | id 중복 여부 |
| message       | string        | X         | id 중복인 경우 오류 메시지             |
## [GET] /users/check-nickname/:nickname
### 설명
+ nickname 중복검사
### request
+ Headers

|Name|Type|Required|Description|
|:-:|:-:|:-:|:-:|
|clientToken|string|O|fe client token|
+ url params

|Name|Type|Required|Description|
|:-:|:-:|:-:|:-:|
|nickname |string|O|중복 검사할 nickname |
+ body
```
no body
```
### response
```json
{
	success: boolean,
	message: string
}
```

|  Name  |  Type   | Required | Description  |
|:------:|:-------:|:--------:|:------------:|
| sucess | boolean |    O     | nickname 중복 여부 |
| message       | string        | X         | id 중복인 경우 오류 메시지             |
