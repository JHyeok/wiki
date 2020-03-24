# Git 작업 중 블루스크린으로 Git 손상 대처 방법

Git 작업중에(Push, Commit, Rebase 등) 컴퓨터 BlueScreen 발생하면, Git의 작업들이 다 손상되어서 Git 작업중이던 파일들이 기존의 작업내역까지 이진 데이터로 변환되면서 읽을 수 없게 손상되어버린다.

Git 관련 명령어들도 `Fatal: not a git repository` 라는 오류가 발생한다.

해결 방법은 `.git` 디렉토리에서 `index`, `ORIG_HEAD`, `HEAD` 파일을 지워야 한다. 그리고 `HEAD` 파일을 새로 가져와야 하는데 아래 해결방법은 `FETCH_HEAD`를 사용했는데 본인은 `FETCH_HEAD`가 보이지 않아서 `refs/heads/master`를 복제해서 `HEAD`로 만들어서 사용했다.

1. index, ORIG_HEAD, HEAD 파일 삭제
2. FETCH_HEAD 또는 refs/heads/master 를 복제해서 HEAD로 이름 변경
3. Git Bash 열고 `git status` 하면 이제 오류가 안뜨고 정상적으로 된다.
하지만 소스파일들은 다 깨져있다.
4. `git reset` 커맨드 입력. rebase 도중에 깨진 사람들은 이거 다음단계까지 진행하여야 하고, 커밋도중에 깨진사람들은 이 명령어까지만 해도 깨진 파일들이 복구된다.
5. `git rebase --abort` 이 명령어 입력 이후 이제 파일들이 복구되었다.

---
#### 참고

https://stackoverflow.com/questions/17722794/fatal-not-a-git-repository-after-bsod

https://stackoverflow.com/questions/26178420/git-rebase-operation-was-interrupted-by-a-pc-crash-now-repository-is-unavailabl