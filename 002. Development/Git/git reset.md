---
tags:
  - 학습정리
  - Git
---
> [!info] 
> 커밋들을 지우며 이전 커밋 상태로 되돌린다.

[[git revert]] 와 다르게 되돌리고자 하는 상태 이후의 커밋들을 삭제한다는 것이 특징이다.
## 사용 예시

```bash
git commit -m "1"
git commit -m "2"
git commit -m "3"
```

위와 같이 3번 커밋을 했다고 가정하자.

```bash
git reset HEAD^
```

이렇게 명령어를 날리면 commit 을 바로 이전 상황으로 되돌린다.
즉, "2" commit 으로 되돌아가는 것이다.

```bash
git reset HEAD~2
```

이렇게 명령어를 날리면 "1" commit 으로 되돌아간다.

```bash
git reset commit-hash
```

이렇게 HEAD~2 부분에 commit hash 를 적어도 되돌아간다.

## reset 옵션
### --hard

- 돌아간 commit 이후의 변경 이력은 모두 삭제한다.

```bash
git commit -m "1"
git commit -m "2"
git commit -m "3"

git reset --hard 1번-commit-hash
git push
```

- 위처럼 실행하면 2, 3번 commit 반영 내용은 완전히 사라진다.
- 코드까지 같이 날아간다는 의미이다.

### --mixed

- 돌아간 commit 이후의 변경 이력은 모두 삭제하지만, 변경 내용은 unStage 상태로 남아있는다.

```bash
git commit -m "1"
git commit -m "2"
git commit -m "3"

git reset --mixed 1번-commit-hash
git add .
git commit -m "mixed commit"
git push
```

- 따라서 add 명령어를 다시 사용하여 stage 상태로 만든 다음 commit 을 해야 변경사항이 적용된다.
- 옵션을 사용하지 않았을 경우 `git reset` 의 기본 동작방식이다.

### --soft

- 돌아간 commit 이후의 변경 이력은 모두 삭제하지만, 변경 내용은 stage 상태로 남아있는다.

```bash
git commit -m "1"
git commit -m "2"
git commit -m "3"

git reset --soft 1번-commit-hash
git commit -m "mixed commit"
git push
```

- 따라서 `add` 명령어 없이 바로 commit 을 사용하여 변경사항을 적용할 수 있다.

## 주의사항
### origin 에 올린 상태에서 reset 하고 push 한다면?

- local 과 origin 의 브랜치 상태가 맞지 않으므로 에러가 발생한다.
- 이를 방지하려면 `--force` 옵션을 주어 강제로 commit history 를 origin commit history 로 덮어씌워야 한다.

## Reference

- https://kyounghwan01.github.io/etc/git/git-reset-revert/#reset