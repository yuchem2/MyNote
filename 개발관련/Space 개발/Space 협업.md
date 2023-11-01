이슈 생성 외 작업은 [[Git 협업 방법]]과 유사

1. 이슈 생성하기
	좌측 메뉴에서 `issue Boards`를 선택한 뒤 오른쪽 상단의 `New issue`를 클릭해 이슈 작성. 이때 Boards는 주간 공유 중 가장 빠른 주차를 선택해 작성

2. Repository Clone
	좌측 메뉴에서 `Repositories`를 선택한 뒤 `clone`하고자하는 repository를 찾아 `clone` 클릭. `ssh`, `https` 중 선호하는 방식을 선택한 후 url을 복사. `ssh` 선택 시 space에 key 등로기 필요(프로필 $\rightarrow$ Security $\rightarrow$ Git Keys)

	원하는 프로젝트 경로에서 다음 명령어를 실행
```shell
git clone ${url}
```
3. Repository Setting
	commit시 사용할 name과 email을 설정
```shell
git config user.name "${이름} ${성}"
git config user.email "${space email}"
```

4. Git branch create
	`clone`이후 작업 수행. 먼저 현재 `branch`가 `main branch`임을 다음 명령어로 확인. 현재 `main branch`가 아닌 겨우 추가적인 명령어로 `main branch`로 변경한 뒤 `branch` 생성 후 작업 진행
```shell
git branch --show-current # branch 확인
git checkout main # branch change
git checout -b ${branch name} # create new branch
```

5. Git Commit 
	작성한 파일을 실제 프로젝트에 반영하기 위해 `commit` 진행
```shell
git add ${file name} 
git commit -m "commit comment"
```

6. Git push
	`commit`을 `remote repository`에 `push`
```shell
git push origin ${working branch name}
```

7. MR 생성 & 리뷰 요청
	space에 올라온 `branch`를 `main branch`로 `merge`하기 위해 MR을 생성. 좌측 메뉴에서 `Code Reviews`를 선택한 뒤 `Create`클릭. `source branch`가 작업을 완료한 `branch`임을 확인하고, title은 다른 사람들이 이해할 수 있도록 작성. 본인이 작업한 MR을 확인해줄 사람들을 Reviewers로 등록. 모든 설정이 끝나면 `Create Merge Request` 버튼을 클릭
	
8. MR과 이슈 연결
	생성된 MR과 이슈를 연결. 연결하고자 하는 `issue` 접속 $\rightarrow$ `Link code changes` 클릭 $\rightarrow$ `Code Reviews`를 클릭한 후 방금 생성한 MR을 클릭하여 연결
	
9. Merge
	한 명 이상의 reviewer의 승인을 받으면 `merge`를 할 수 있음. reviewer가 accept를 하면 확인 가능하고, accept 이후에 `merge` 버튼을 클릭해 수행. `merge`를 할 때에는 `rebase and squash`를 사용. 
	`merge` 이후에 해당 이슈를 `resolve`
	
10. local 소스 코드 최신화
	수정한 사항을 local에 반영
```shell
git checkout main
git pull origin main
```