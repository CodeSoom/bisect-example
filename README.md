# Git bisect 예제

## 요구사항

nodejs가 설치되어 있어야 합니다.

## 실행 방법

실행하는 순서와 결과를 같이 출력했습니다.

```bash
$ node app.js # 에러 발생
/bisect-test/app.js:19
  .filter((user) => use.age >= 30)
                            ^
ReferenceError: use is not defined
    at /bisect-test/app.js:19:29
```

```bash
$ git bisect start # bisect 시작
status: waiting for both good and bad commits
```

```bash
$ git bisect bad # 현재 커밋을 에러나는 커밋으로 지정
status: waiting for good commit(s), bad commit known
```

```bash
$ git bisect good bf5a3c6 # bf5a3c6 First commit을 에러안나는 커밋으로 지정
Bisecting: 1 revision left to test after this (roughly 1 step)
[cd82e14fe34b396884f6a5a9ba9e5cf29df4608c] 미국에 사는 사람들만 출력해라
```

```bash
$ node app.js # 에러없이 정상적으로 실행되는 것 확인
John
Bob
James
Lily
Daniel
Olivia
```

```bash
$ git bisect good
Bisecting: 0 revisions left to test after this (roughly 0 steps)
[d5dd426b79b7e2fc45415aff341ea80bc7b7e7fa] 나이가 30살 이상인 사람만 불러와라
```

```bash
$ node app.js # 에러 발생하는 것 확인
/bisect-test/app.js:19
  .filter((user) => use.age >= 30)
                            ^
ReferenceError: use is not defined
    at /bisect-test/app.js:19:29
```

```bash
$ git bisect bad
d5dd426b79b7e2fc45415aff341ea80bc7b7e7fa is the first bad commit
commit d5dd426b79b7e2fc45415aff341ea80bc7b7e7fa
Author: User <user@gmail.com>
Date:   Wed Sep 25 20:00:59 2024 +0900

    나이가 30살 이상인 사람만 불러와라

 app.js | 5 +++--
 1 file changed, 3 insertions(+), 2 deletions(-)
```

```bash
$ git bisect reset
Previous HEAD position was d5dd426 나이가 30살 이상인 사람만 불러와라
Switched to branch 'main'
```

## bisect run을 이용한 자동화

```bash
git bisect start # bisect 시작
git bisect bad # 현재 커밋을 에러나는 커밋으로 지정
git bisect good bf5a3c6 # bf5a3c6 First commit을 에러안나는 커밋으로 지정
git bisect run node app.js # 각 커밋에서 해당 스크립트를 실행해 에러가 나면 bad, 안나면 good으로 자동 실행
git bisect reset # 초기화
```
