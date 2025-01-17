각 함수는 [api.js](api.js.md)의 `getKeywordGraphs()`, `getNodes()`, `getHierarchy()`, `getLabels()`를 호출하여 forge에서 제공하는 storage에 저장

각 함수는 `mainfest.yml`에서 1시간 간격(최소)으로 호출되어 데이터를 주기적으로 업데이트 진행
```
...
scheduledTrigger:  
  - key: keyword-scheduled-trigger  
    function: keywordTrigger  
    interval: hour # Runs hourly  
  - key: hierarchy-scheduled-trigger  
    function: hierarchyTrigger  
    interval: hour # Runs hourly  
  - key: nodes-scheduled-trigger  
    function: nodesTrigger  
    interval: hour # Runs hourly  
  - key: labels-scheduled-trigger  
    function: labelsTrigger  
    interval: hour # Runs hourly
...
```
