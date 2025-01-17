### `getKeywordGraphs()`
confluence의 모든 페이지 데이터를 검색해 각 문서의 키워드를 추출하고, 키워드와 관련된 문서 간의 연결 그래프를 생성

완성된 그래프는 다음 정보를 가지고 있음. 
```json
{
	nodes: [
		{
			id: d.id,  
			title: d.title,  
			// body: body,  
			keywords: keywords,  
			searched: false,  
			url: docUrl,  
			authorName: authorName,  
			status: d.status,  
			createdAt: createdAt
		}
	],
	links: [
		{
			source: id,  
			target: d.id,  
			type: 'keyword',
		}
	]
}
```
### `getNodes()`
confluence의 모든 페이지 데이터를 검색해 문서 목록 반환. `getKeywordGraphs()`의 `nodes`와 동일한 형태의 노드를 만들어 리턴
### `searchByAPI(searchWord)`
특정 키워드를 통해 confluence 페에지를 검색해 page id만 리턴
### `getUserInfo(accountId)`
Confluence 계정 ID로 사용자 이름 조회
### `getHierarchy()`
Confluence의 모든 페이지의 계층적 형태에 따라 그래프를 생성. 이때 `getAllRootPagesInSpace()`와 `getAllChildrenPages()` 이용. 문서간의 관계(`links`)만 리턴.
```json
{
	links: [
		{
			source: childPage.parentId,  
			target: childPage.id,  
			type: 'hierarchy',
		}
	]
}
```
#### `getAllRootPagesInSpace(spaceId, depth = 'root')`
주어진 공간(`spaceId`)의 모든 루트 페이지를 검색한다. 
#### `getAllChildrenPages(pageId)`
특정 페이지의 모든 하위 페이지를 검색. 페이지 정보에 부모 페이지 정보를 포함하도록 해 리턴
### `getLabels()`
Confluence의 모든 페이지를 레이블을 기반으로 그래프 생성. 문서간의 관계(`links` )만 리턴
```json
[
	{
		source: response.results[i].id,  
		target: response.results[j].id,  
		type: 'labels',
	}
]
```
### `fetchPageInfos()`
Confluence의 특정 페이지의 기본 정보를 가져옴. 
```json
[
	{
		id: d.id,  
		title: d.title,  
		content: content
	}
]
```
### `getDocumnetInfo(pageId)`
Confluence의 특정 페이지(`pageId`)를 사용해 문서 상세 정보를 가져옴.
```json
{
	title: data.title,  
	url: data._links.base + data._links.webui,  
	status: data.status,
	authorName: data.history.createdBy.username,
	createdAt: data.createdDate,  
}
```
