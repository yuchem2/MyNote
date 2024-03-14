## [GET] /maps/:mapId/memories
### 설명
+ mapId에 속한 memory를 가져옵니다
### request
+ Headers 

| Name | Type | Required | Description |
| :--: | :--: | :--: | :--: |
| clientToken | string | O | fe client token |
| Authorization | string | O | login 시 발급받은 token |

+ URIparams

| Name  |  Type  | Required | Description |
|:-----:|:------:|:--------:|:-----------:|
| mapId | string |    O     | map 식별자  |
### response
```json
{
	memorise: [
		{
			_id: string,
			title: string,
			content: string,
			startDate: datetime, 
			endDate: datetime,
			createdAt: datetime,
			updatedAt: datetime,
		},
	],
}
```

| Name | Type | Required | Description |
|:----:|:----:|:--------:|:-----------:|
| memories     | [MemoryInfo]     | O         | memory 리스트            |
+ memoryInfo

|   Name    |   Type   | Required |  Description  |
|:---------:|:--------:|:--------:|:-------------:|
|    _id    |  string  |    O     | memory 식별자 |
|   title   |  string  |    O     |  memory 제목  |
|  content  |  string  |    O     |  memroy 내용  |
| startDate | datetime |    O     | memory 시작일 |
|  endDate  | datetime |    O     | memory 종료일 |
| createdAt | datetime |    O     | memory 생성일 |
| updatedAt          | datetime         | O         | memory 마지막 수정일              |
## [POST] /maps/:mapId/memories
### 설명
+ mapId에 속한 memory를 생성합니다
### request
+ Headers 

| Name | Type | Required | Description |
| :--: | :--: | :--: | :--: |
| clientToken | string | O | fe client token |
| Authorization | string | O | login 시 발급받은 token |

+ URIparams

| Name  |  Type  | Required | Description |
|:-----:|:------:|:--------:|:-----------:|
| mapId | string |    O     | map 식별자  |

+ Body
```json
{
	title: string,
	content: string,
	startDate: datetime,
	endDate: datetime,
}
```
### response
```
http 204 status code
```

## [PUT] /maps/:mapId/memories/:memoryId
### 설명
+ memory를 수정합니다
### request
+ Headers 

| Name | Type | Required | Description |
| :--: | :--: | :--: | :--: |
| clientToken | string | O | fe client token |
| Authorization | string | O | login 시 발급받은 token |

+ URIparams

| Name | Type | Required | Description |
| :--: | :--: | :--: | :--: |
| mapId | string | O | map 식별자 |
| memoryId | string | O | memory 식별자 |

+ Body
```json
{
	title: string,
	content: string,
	startDate: datetime,
	endDate: datetime,
}
```
### response
```
http 204 status code
```

## [DELETE] /maps/:mapId/memories/:memoryId
### 설명
+ memory를 삭제합니다
### request
+ Headers 

| Name | Type | Required | Description |
| :--: | :--: | :--: | :--: |
| clientToken | string | O | fe client token |
| Authorization | string | O | login 시 발급받은 token |

+ URIparams

| Name | Type | Required | Description |
| :--: | :--: | :--: | :--: |
| mapId | string | O | map 식별자 |
| memoryId | string | O | memory 식별자 |
### response
```
http 204 status code
```

