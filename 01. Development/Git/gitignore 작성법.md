---
tags:
  - Git
---

```yaml
# 폴더 전체 무시하기
MyFolder/ 

# 특정 확장자 전체 무시하기
*.log

# 특정 디렉토리의 파일 무시하기
MyFolder/file.log




```

```yaml
# 이미 Staging Area 나 Repository 에 올라간 파일을 무시하기
git rm [파일명]
git commit -m [메시지]
```

```yaml
# 일부 파일에 대해서만 ignore 를 해제하고 싶다면
## 별도 라인에 ignore 처리된 폴더 자체를 먼저 예외처리 시키고, 
## 해당 폴더 내의 모든 파일들을 * 을 이용하여 ignore 처리한 이후에,
## 최종적으로 다시 파일들을 예외 처리하여 Git 에 포함시키는 것이다.

!folder/
folder/*
!folder/*.txt
```
