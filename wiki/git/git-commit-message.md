# Git Commit 메시지

## Git Commit 메시지 포맷

Git을 처음 접했을 때는 혼자서만 사용하는 용도였기 때문에 커밋 메시지에 신경을 쓰지 않았다.

하지만 회사에서 Git을 사용하고 규칙이 정해지지 않은 상태에서의 Git 이용은 좋지 않다고 생각한다. 다른 사람이 무엇을 수정해서 커밋했는지 명확히 알기 힘들고 이전 커밋으로 돌아가기에도 무분별한 커밋은 비효율적이고 일의 능률을 떨어트린다고 생각한다.

그래서 아래와 같이 커밋 메시지 규칙을 정하였고 만약 커밋 메시지에서 README.md를 수정해서 README.md를 수정했다고 커밋을 한다면 README.md 수정된 부분만 커밋을 올려야지 Test.cs가 수정된 부분까지 같이 올리면 안된다.

``` xxx 기능 추가 ```

``` Add xxxx ```

위는 기존에 사용하던 커밋 메시지 방식이었다.

다른 사람들의 깃허브 프로젝트의 커밋을 보다가 나름대로 정리가 잘된 것이 있어서 앞으로 나도 깃허브에 저장하는 프로젝트들의 커밋 메시지는 아래의 규칙을 따르려고 한다.
이것은 개인의 규칙이며, 팀에서 적용을 하기 위해서는 팀원들끼리 협의가 필요하다고 생각한다.

- feat: (새로운 기능)
- fix: (버그 수정)
- docs: (문서 변경)
- style: (서식 수정, 세미콜론 추가, 기타; 코드 변경 없음)
- refactor: (코드 리팩토링)
- test: (누락된 테스트 추가, 리팩토링 테스트; 프로덕션 코드 변경 없음)
- chore: (기타 업데이트, 패키지 설치 등 잡다한 작업; 프로덕션 코드 변경 없음)

예를들면

```
fix: CarService의 색변경 로직 오류 수정

CarService의 잘못된 변수 'CarColor' 를 삭제
```

description 을 적고 옵션으로 밑에 세세한 내용을 적는다.

기존에 TIL은 기존 규칙대로 커밋을 하기로 했다. 레포지토리에서 커밋이 이미 상당수 진행된 상태에서 커밋 규칙이 바뀌는 것은 별로 좋지 않다고 본다.

## Push 된 Git Commit 메시지 수정

```
git commit --amend -m "메시지 수정"
```

`--amend` 옵션을 이용해서 수정한 이후에 변경내용을 원격 브랜치에 적용한다.

```
git push [remote] [branch] --force
```
```
git push <remote> [branch] -f
```

## Git Commit 메시지 취소 및 Push 취소

```
# 방금 커밋을 했을 때, 취소하고 다시 unstaged 상태로 오는 명령어
git reset --mixed HEAD^

# 가장 최근의 Push된 커밋을 취소하고 다시 unstaged 상태로 오는 명령어
git reset HEAD^
```

---
#### 참고

https://www.conventionalcommits.org/en/v1.0.0-beta.2/

http://karma-runner.github.io/0.10/dev/git-commit-msg.html