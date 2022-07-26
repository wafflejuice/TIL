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
