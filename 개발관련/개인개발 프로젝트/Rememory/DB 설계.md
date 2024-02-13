## User
```json
{
	_id: ObjectId,
	clientId: string,
	passwd: string, // token 암호화 필요
	oauthProvider: string, // OAuth 제공자(google, kakao)
	createdAt: datetime,
	updatedAt: datetime,
}
```
## Map
```json
{
	_id: ObjectId,
	name: string, // 지도 이름
	category: string, // 지도 구성 속성
	parentMap: ObjectId, // 상위 지도 식별자
	maps: [ObjectId, ], // 하위 지도 목록
}
```
## Memory
```json
{
	_id: ObjectId,
	owner: ObjectId, // 소유한 사용자 식별자
	mapId: ObjectId, // memory가 작성된 지도 식별자
	title: string,
	content: string,
	startDate: datetime, // 사용자가 지정한 여행 시작 날짜
	endDate: datetime, // 사용자가 지정한 여행 종료 날짜
	createdAt: datetime,
	updatedAt: datetime, 
}
```
