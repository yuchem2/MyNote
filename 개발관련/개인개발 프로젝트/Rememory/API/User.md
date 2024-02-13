## [POST] /signup
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
	id: string,
	password: string,
	nickname: string,
}
```
### response
```
http 204 status code
```
## [POST] /login
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
```json
{
	jwt: string,
	nickname: string,
}
```

|  Name   |  Type  | Required |     Description     |
|:-------:|:------:|:--------:|:-------------------:|
| jwt | string |    O     | 발급받은 jwt |
| nickname        | boolean       | O         | 로그인한 사용자 이                    |
## [POST] /logout
### 설명
+ 로그인된 계정을 로그아웃합니다
### request
+ Headers

| Name | Type | Required | Description |
| :--: | :--: | :--: | :--: |
| clientToken | string | O | fe client token |
| Authorization | string | O | login 시 발급받은 token |
+ body
```
no body
```

### response
```
http 204 status code
```