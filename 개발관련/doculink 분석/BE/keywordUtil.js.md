### `getKeywordGraphs(documents)`
`documents`를 입력받아 해당 document들의 keyword들을 분석해 연관도를 통해 links를 생성. [api.js](api.js.md)의 `getKeywordGraphs()`에서 links 추출과정과 동일. `keyword`를 추출하는 과정 제거된 상태.
```json
[
	{
		source: id,  
		target: document.id,  
		type: 'keyword',
	}
]
```