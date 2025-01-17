Atlassian Forge 환경에서 API 요청을 처리하는 Resolver를 정의한 코드. 

각 `functionKey`에 따라 작동해야 하는 기능을 정의.
### `getNodes`
`storage`에서 `nodes`를 리턴. 만약 데이터가 없는 경우 [api.js](api.js.md)의 `getNodes()`를 호출하여 데이터를 가져온 뒤 `storage`에 저장.
### `getKeywordGraphs`
`storage`에서 `keword`를 리턴. 만약 데이터가 없는 경우 [api.js](api.js.md)의 `getKeywordGraphs()`를 호출하여 데이터를 가져온 뒤 `storage`에 저장.
### `getHierarachy`
`storage`에서 `hierarchy`를 리턴. 만약 데이터가 없는 경우 [api.js](api.js.md)의 `getHierarchy()`를 호출하여 데이터를 가져온 뒤 `storage`에 저장.
### `getLabels`
`storage`에서 `labels`를 리턴. 만약 데이터가 없는 경우 [api.js](api.js.md)의 `getLabels()`를 호출하여 데이터를 가져온 뒤 `storage`에 저장.
### `searchByAPI`
`req.payload`에서 `searchWord`를 받아 해당 변수를 통해 [api.js](api.js.md)의 `searchByAPI()`를 호출하여 검색 후 결과를 리턴
### `sync`
초기 접속할 때 호출되는 `functionKey`로 [api.js](api.js.md)에서 각 함수를 호출하여 `nodes, keyword, hierarchy, labels`를 `storage`에 저장하고, 그 결과를 리턴