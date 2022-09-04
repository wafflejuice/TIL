# 2022-07-26
## github pull request를 local로 가져오기
코드 리뷰를 하다보면 local에서 보고싶을 때가 있다.
`.git/config`에서 다음과 같이 추가
```
[remote "origin"]
    ...
    fetch = +refs/pull/*/head:refs/remotes/origin/pr/*
```

이후 `git pull --all` 하면 잘 가져와진다.

master와 비교해 변경사항을 보고싶다면
```
git diff origin/master origin/pr/{#pr}
```

변경 line 위아래 3줄씩밖에 보이지 않는 게 불편하다면

```
git diff origin/master origin/pr/{#pr} -U1000
```

[GitHub의 Pull Request를 로컬로 가져오기](https://blog.outsider.ne.kr/1204)

이대로 보기에는 눈에 잘 들어오지 않으므로 diff-so-fancy를 install하여 사용한다.

[diff-so-fancy](https://github.com/so-fancy/diff-so-fancy.git)

# 2022-09-04
## Git LFS(Large File Storage)
100Mb 초과 file을 Github remote server에 우회 저장할 수 있도록 해준다. (Github은 기본적으로 100Mb 초과 file push 금지)  
large file을 Large File Storage에 저장되고 Github remote server에는 text pointer만 존재하는 방식으로 동작한다.
