
[[Lexical Analyzer(Scanner)]]와 [[Syntax Analyzer(Parser)]]가 identifier에 관한 정보를 수집하여 저장해 놓은 테이블

Semantic anaylsis와 Code generation시에 사용된다. 즉 [[Intermediate Code Generator]]에 의해 사용된다

name과 attributes가 저장되며 일반적으로 name을 index로 하는 [[Hash Table]]의 형태를 가진다