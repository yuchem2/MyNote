### `fetchConfluencePages(payload)`
[api.js](api.js.md)의 `fetchPageInfos()`의 결과를 리턴
### `registerKeywords(payload)`
`payload.document`를 입력으로 하여 [keywordUtil.js](keywordUtil.js.md)의 `getKeywordGraps()`의 결과를 `storage`에 `rovo`에 저장함

해당 함수는 `mainfest.yml`에서 `rovo`에 의해 호출되어 사용됨. rovo가 모든 페이지를 정보를 가져온 후 keyword를 추출한다. 그 후 `registerKEywords(payload)`를 통해 문서간의 keyword 연관도를 계산. 
```
...
function:
...
	- key: fetchConfluencePages  
	  handler: index.fetchConfluencePages  
	- key: registerKeywords  
	  handler: index.registerKeywords
...
rovo:agent:    
  - key: keyword-extractor-agent  
    name: Keyword Extractor Agent  
    description: >  
      An agent for extracting key keywords from Confluence pages.  
    prompt: >  
      Role: You are responsible for analyzing documents and extracting key keywords to help users easily understand the content.  
  
      You can perform the following jobs based on the user's request:  
  
      a. Extract keywords from a list of Confluence pages  
  
      I'll separate the instructions for each job with a '---' on a new line, followed by the job title.  
  
      ---  
  
      a. Extract keywords from a list of Confluence pages  
  
      To do this, follow these steps:  
  
      1. Fetch the text of the Confluence pages from all Confluence pages using the 'fetch-confluence-pages' action.  
      2. Extract the keywords from the text of all Confluence pages according to the user's request.  
      3. Structure your response as follows:  
         i. Response should be in json format. Json should have "documents" field. "documents" field should have "id" and "keywords" fields. Json format should be following format:  
         {  
          "documents": [  
            {  
              "id": Page Id,  
              "keywords": [Extracted Keywords],  
            },  
            ...  
          ]  
          }  
         Follow these rules:  
          - Do not mention specific issue details unless asked by the user.  
      4. Return the response to the user.   
        
      ----  
  
      b. Register keywords  
  
      To do this, follow these steps:  
  
      1. Perform the 'register-keywords' action to register each of the Confluence page ID and keywords created in the previous step according to the user's request.  
      2. Return the result to the user.   
        
      ----  
  
    conversationStarters:  
      - Extract and register keywords for all Confluence pages  
  
    actions:  
      - fetch-confluence-pages  
      - register-keywords
action:   
  - key: fetch-confluence-pages  
    name: fetch-confluence-pages  
    function: fetchConfluencePages  
    actionVerb: GET  
    description: >  
      This action fetches the text of Confluence pages.  
  - key: register-keywords  
    name: register-keywords  
    function: registerKeywords  
    actionVerb: CREATE  
    description: >  
      This action registers keywords.  
    inputs:  
      documents:  
        title: Documents  
        type: string  
        required: true  
        description: |  
          "The list of the all Confluence pages with page id and keywords"
...
```

