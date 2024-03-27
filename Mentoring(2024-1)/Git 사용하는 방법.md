> 프로젝트 파일을 clone 하거나 하는 경우 폴더의 경로에 한글이 포함되지 않는 경로를 사용하세요.
> + O : `C:/document/program/programing-study-2024-1`
> + X: `C:/윤재현/program/programing-study-2024-1`

1. 명령 프롬포트를 연다.
   ![500](Pasted%20image%2020240327211906.png)
2. clone할 경로를 확인한다. 해당 화면에서는 `C:\Program`
   ![500](Pasted%20image%2020240327212147.png)
3. 프롬포트에서 경로를 이동한다. `cd C:\program`
   ![500](Pasted%20image%2020240327212903.png)
4. 초대된 Github organization으로 이동한 뒤 programing-study-2024-1 organization으로 이동한다. (Github의 자신의 프로필을 누르면 your organization이 나옵니다.)
   ![500](Pasted%20image%2020240327212548.png)
5. code를 누른 후 httl url을 복사한다.
   ![500](Pasted%20image%2020240327212648.png)
6. git clone하기 `git clone https://github.com/coding-mentoring-ku/programing-study-2024-1.git`
   ![500](Pasted%20image%2020240327212844.png)
7. 폴더를 이동한 뒤 확인 `cd programing-study-2024-1` -> `git status`
   ![500](Pasted%20image%2020240327213347.png)
8. commit 시 사용할 자신의 이름과 메일을 설정합니다.
	`git config user.name "이름"`
	`git config user.email "email"`
![500](Pasted%20image%2020240327213536.png)
9. 이제 visual studio를 켜서 해당 파일을 킵니다. 
   ![500](Pasted%20image%2020240327213659.png)![500](Pasted%20image%2020240327213717.png)
10. 창이 켜지면 보기 - 터미널을 통해 cmd를 엽니다. (창이 안 열려있는 경우)
    ![500](Pasted%20image%2020240327213823.png)
11. 기본적으로 powershell이 켜지므로 눌러서 명령 프롬포트로 변경합니다. 
    ![500](Pasted%20image%2020240327214109.png)
12. 해당 창에서 `git status`를 입력합니다.
    ![500](Pasted%20image%2020240327214211.png)
13. 해당 문구가 나오면 `git config --global --add safe.directory ${파일경로}`를 입력합니다. 해당 예시에서는 `git config --global --add safe.directory C:/Program/programing-study-2024-1` 다음과 같이 입력합니다. 
    문구를 복사하고 싶으면 드래그 한 뒤 마우스 오른쪽 클릭을 하며 붙여넣기를 할 때도 동일합니다. 
    ![500](Pasted%20image%2020240327214403.png)
14. 위 명령어가 적용되면 `git` 명령어들이 정상적으로 작동합니다. 
    ![500](Pasted%20image%2020240327214442.png)
15. `git checkout -b ${브랜치 이름}`을 입력합니다. `${}` 안의 내용은 `날짜_이름` 형식으로 작성해주시길 바랍니다.  (영어로)
    ![500](Pasted%20image%2020240327220030.png)
16. 오른쪽에 파일에서 자신의 이름을 선택한 뒤 하고자 하는 작업을 수행합니다. 그 후 `git status`를 입력합니다.
    ex) 새파일 만들기, 코드 수정하기, 등등
    ![500](Pasted%20image%2020240327220046.png)
16. `git add .`을 입력합니다. `git status`를 통해 변경된 것을 확인할 수 있습니다.
    ![500](Pasted%20image%2020240327220105.png)
17. `git commit -m ${커밋 메시지}`를 입력합니다. `${}`안의 내용은 자유롭게 적으시면 됩니다. (영어로) 
    ex) `git commit -m "add file"`, `git commit -m "submit works"`, `git commit -m "push commit"` 등등
    ![500](Pasted%20image%2020240327220127.png)
18. 그 후 원격 저장소에 저장하기 위해 branch를 push합니다. `git push origin ${자신의 브랜치이름}`
    ![500](Pasted%20image%2020240327220334.png)
19. 아까 repository url을 복사한 사이트에 다시 접속하면 자신의 branch가 동기화된 것을 확인할 수 있습니다.
    ![500](Pasted%20image%2020240327220519.png)
20.  상단의 pull request를 클릭하여 이동합니다.
    ![500](Pasted%20image%2020240327220555.png)
21. 초록색 버튼을 클릭한 후 자신의 branch를 왼쪽에서 선택합니다.
    ![500](Pasted%20image%2020240327220633.png)
22. 클릭을 하게 되면 자신이 수정한 사항들을 확인할 수 있습니다. 
23. 초록색 create pull request를 클릭해 pull request를 생성합니다. 아래 화면이 나오면 다시 한번 클릭해 생성하시면 됩니다.
    ![500](Pasted%20image%2020240327220800.png)![500](Pasted%20image%2020240327220822.png)
24. 아래 화면에서 assigness에서 자기 자신을 클릭해주시고, reviewers로 yuchem2를 선택해주시면됩니다. (윤재현 멘토)
    ![500](Pasted%20image%2020240327220935.png)
25. 그러면 제가 approve를 하는 경우 merge 버튼이 활성화됩니다. 위 그림에서는 활성화가 되어있지만, 다른 분들은 reviews가 필요하다는 메시지가 출력될 것입니다.
26. merge가 완료되었으면 다시 visual studio로 돌아갑니다. 돌아간 뒤 `git checkout main`을 입력합니다. 
    ![500](Pasted%20image%2020240327221414.png)
27. 그 후 동기화를 위해 `git pull origin main`을 입력합니다. 그러면 merge된 자신의 코드를 `main` 브랜치에서 확인할 수 있습니다.
    ![500](Pasted%20image%2020240327221458.png)
28. `git branch`를 입력해 branch 목록을 확인한 후 자신이 방금 merge한 브랜치를 삭제합니다. `git branch` -> `git branch -D ${삭제할 브렌치이름}`
    ![500](Pasted%20image%2020240327221631.png)
29. 새로운 코드를 작성해 반영하기 위해 15~29를 반복하시면 됩니다.  