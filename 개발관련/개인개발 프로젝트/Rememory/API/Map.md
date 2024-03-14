## [GET] /maps
### 설명
+ 메인 화면의 전체 나라 map 데이터를 가져옵니다
### request
+ Headers

| Name | Type | Required | Description |
| :--: | :--: | :--: | :--: |
| clientToken | string | O | fe client token |
| Authorization | string | O | login 시 발급받은 token |
### response
+ Body
```json
{
	maps: [
		{
			_id: string,
			name: string,
			category: string,
			maps: [
				{
					_id: string,
					name: string,
					category: string,
				},
			],
		
		},
	]
}
```

| Name  |  Type  | Required | Description |
|:-----:|:------:|:--------:|:-----------:|
| maps | [mapInfo] |    O     | map 리스트   |
+ mapInfo

|   Name   |  Type  | Required |            Description            |
|:--------:|:------:|:--------:|:---------------------------------:|
|   _id    | string |    O     |            map 식별자             |
|   name   | string |    O     |             map 이름              |
| category | string |    O     | map 분류(country, city, district) |
|   maps   | [mapInfo]       | O         | 하위 map 리스트                                  |

## [GET] /maps/:mapId
### 설명
+ 각 map에 소속되어 있는 map 데이터를 가져옵니다
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
+ Body
```json
{
	_id: string,
	name: string,
	category: string,
	parentMap: {
		_id: string,
		name: string,
	},
	maps: [
		{
			_id: ObjectId,
			name: string,
			category: string,
		},
	]
}
```

| Name | Type | Required | Description |
| :--: | :--: | :--: | :--: |
| _id | string | O | map 식별자 |
| name | string | O | map 이름 |
| category | string | O | map 분류(country, city, district) |
| parentMap | [parentMapInfo] | O | 상위 map 정보 |
| maps | [mapInfo] | O | 하위 map 정보 |
+ parentMapInfo

| Name |  Type  | Required | Description |
|:----:|:------:|:--------:|:-----------:|
| _id  | string |    O     | map 식별자  |
| name | string |    O     |  map 이름   |
+ mapInfo

|   Name   |  Type  | Required |            Description            |
|:--------:|:------:|:--------:|:---------------------------------:|
|   _id    | string |    O     |            map 식별자             |
|   name   | string |    O     |             map 이름              |
| category | string |    O     | map 분류(country, city, district) |
