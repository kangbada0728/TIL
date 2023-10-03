---
tags:
  - 학습정리
  - Git
---
> [!info]
> 새로운 커밋을 추가하며 이전 커밋을 취소한다.

즉, 데이터를 추가하는 방식으로 코드를 취소하는 것이다.

[[git reset]] 과 다르게 커밋을 삭제하지 않는다.
`Revert "..."` 라는 커밋이 추가된다.

## 사용 예시

```bash
git commit -m "1"
git commit -m "2"
git commit -m "3"

git revert 1번-commit-hash
```

위처럼 명령어를 실행하면 `1번 커밋` 이후의 커밋들이 삭제되는 것이 아니라, `1번 커밋` 에 해당하는 내용만 삭제된다.

그리고 `Revert "1번 커밋"` 이라는 커밋에는 1번 커밋이 삭제된 이력이 남게 된다.

`git log` 에는 다음과 같이 나온다.
```bash
Revert "1번 커밋"
3번 커밋
2번 커밋
1번 커밋
```

## --no-commit 옵션

만약 revert 한 결과를 stage 상태만 유지하고, commit 하지 않으려면 `--no-commit` 옵션을 추가하면 된다.

```bash
git revert --no-commit 커밋-해시
git commit -m "revert 한 커밋내용"
git push
```

## 여러 개의 커밋을 되돌리려면?

```bash
git revert [1번커밋해쉬]..[2번커밋해쉬]
git log

Revert "2번커밋해쉬"
Revert "1번커밋해쉬"
```

## [[git reset]] 대신 git revert 를 써야하느 ㄴ이유는?

revert 는 되돌리는 커밋이 중간에 있을 때 커밋 해시를 넣어서 중간 커밋만 삭제할 수 있고, 어떤 커밋이 왜 revert 됐는지 commit message 를 통해 확인할 수 있어 유용하다.

또한 revert 이력까지 관리할 수 있기 때문에 history 유지 차원에서 더 좋다.

## Reference

- https://kyounghwan01.github.io/etc/git/git-reset-revert/#revert