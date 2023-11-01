1. Fork
	프로젝트 repositories에서 `Fork` 버튼을 눌러 자신의 repository로 가져온다.
	
2. Clone 및 Remote
	자신의 repository에 `Fork`된 repository에서 url을 복사한 후 local에서 `clone`
```shell
git clone ${url}
```

3. Git branch create
	`clone`이후 작업 수행. 먼저 현재 `branch`가 `main branch`임을 다음 명령어로 확인. 현재 `main branch`가 아닌 겨우 추가적인 명령어로 `main branch`로 변경한 뒤 `branch` 생성 후 작업 진행
```shell
git branch --show-current # branch 확인
git checkout main # branch change
git checout -b ${branch name} # create new branch
```

4. Git Commit 
	작성한 파일을 실제 프로젝트에 반영하기 위해 `commit` 진행
```shell
git add ${file name} 
git commit -m "commit comment"
```

5. Git push
	`commit`을 `remote repository`에 `push`. 간혹 `push` 과정에서 token access error가 발생하는 경우가 있음. 이 경우 `Setting` $\rightarrow$ `Developer Settings` $\rightarrow$ `Personal access tokens`을 발급 받아야 함
```shell
git push origin ${working branch name}
```

6. PR 요청
	자신의 repository로 들어가면 `Comapare & pull request` 버튼이 활성화되어있음. 해당 버튼을 클릭해 메시지를 작성하고, PR 전송
	
7. Code Review & Merge PR
	원본 repository 관리자는 PR을 받으면 변경 내역을 확인하고 `merge` 여부를 결정. 
	
8. Merge 이후 동기화
	원본 repositroy에서 `merge`가 완료되면 로컬 `main branch`와 원본 repository의 코드를 동기화해야 한다. 자신의 repository에서 `Sync Fork` $\rightarrow$ `Update branch` 

9. local 저장소 동기화
```shell
git checkout main
git pull origin main

# local branch 강제 삭제
git branch -D ${branch name}

# remote branch 삭제
git push origin --delete ${branch name}
```
