# Git-flow

소스코드를 관리하고 출시하기 위한 브랜치 관리 전략 중 하나.

Git이 활성화되기 시작하는 시기에 `Vincent Driessen` 가 블로그 글에서 제안한 workflow 디자인을 기반으로 만들어져, 현재 많은 기업에서 Git으로 개발할 때 표준으로 사용하는 개발 전략이다.

## Git-flow 장점

1. 독릭적인 개발 환경을 만들 수 있다.  <br/>
Gitflow 사용시 기능(티켓) 단위로 독립적인 Branch를 만들기 떄문에 다른사람의 개발 결과에 영향을 받지 않는 독립적인 개발환경을 만들어준다. 이는 최소한의 방해로 개발을 할 수 있게 도와준다.

2. 추적이 쉽다. <br/>
Gitflow의 feature 브랜치는 칸반 보드의 티켓과 연동된다. 칸반 보드를 보고 어떤 기능이 통합되었는지 확인 가능하며, 오류 발생시 어떤 기능을 개발하다가 문제가 발생하였는지 확인하고 쉽게 UNDO 할 수 있다.

3. Branch 별로 역할이 분리되어있기 때문에 각 Branch의 update에 맞춰 배포 & 테스트를 하기에 용이하다.

## Git-flow 전략

Git-flow는 5가지 브랜치를 사용한다.

![image](https://user-images.githubusercontent.com/75151848/205563815-99538ef4-4a4e-4add-95b2-1d9849547305.png)

### 브랜치 종류

주요(main) 브랜치인 `master` , `develop` 과 보조(supporting) 브랜치인 `feature` , `release` , `hotfix` 가 있다.

- master : 제품으로 출시될 수 있는 브랜치
- develop : 다음 출시 버전을 개발하는 브랜치
- feature : 기능을 개발하는 브랜치
- release : 이번 출시 버전을 준비하는 브랜치
- hotfix : 출시 버전에서 발생한 버그를 수정 하는 브랜치

#### feature 브랜치

- 갈라져 나온 브랜치: develop
- 다시 merge할 브랜치: develop
- 브랜치 이름 규칙: master, develop, release-*, hotfix-*를 제외한 것

```shell
# develop 브랜치에서 feature 브랜치 만들기
$ git checkout -b myfeature develop
Switched to a new branch "myfeature"

# 완성된 기능을 develop에 합치기
$ git checkout develop
Switched to branch 'develop'
$ git merge --no-ff myfeature
Updating ea1b82a..05e9557
(Summary of changes)

# 브랜치 삭제
$ git branch -d myfeature
Deleted branch myfeature (was 05e9557).
$ git push origin develop
```

- `--no-ff` 옵션: 항상 merge 커밋을 만들어 merge한다.

#### release 브랜치

- 갈라져 나온 브랜치: develop
- 다시 merge할 브랜치: develop, master
- 브랜치 이름 규칙: release-*

1.2 버전 배포 예시

```shell
# develop 브랜치에서 release 브랜치 만들기
$ git checkout -b release-1.2 develop
Switched to a new branch "release-1.2"

# 버전 넘버가 들어있는 파일 모두 수정하는 가상의 스크립트 실행
$ ./bump-version.sh 1.2
Files modified successfully, version bumped to 1.2.

# 커밋
$ git commit -a -m "Bumped version number to 1.2"
[release-1.2 74d9424] Bumped version number to 1.2
1 files changed, 1 insertions(+), 1 deletions(-)

# master 브랜치에 merege
$ git checkout master
Switched to branch 'master'
$ git merge --no-ff release-1.2
Merge made by recursive.
(Summary of changes)
$ git tag -a 1.2

# develop 브랜치에 merge
$ git checkout develop
Switched to branch 'develop'
$ git merge --no-ff release-1.2
Merge made by recursive.
(Summary of changes)

# 브랜치 삭제
$ git branch -d release-1.2
Deleted branch release-1.2 (was ff452fe).
```

#### hotfix 브랜치

- 갈라져 나온 브랜치: master
- 다시 merge할 브랜치: develop, master
- 브랜치 이름 규칙: hotfix-*

 현재 운영 버전이 1.2이고 심각한 버그가 발견된 경우 예시

 ```shell
# master 브랜치로부터 hotfix 브랜치 만들기
 $ git checkout -b hotfix-1.2.1 master
Switched to a new branch "hotfix-1.2.1"
$ ./bump-version.sh 1.2.1 # 버전 변경
Files modified successfully, version bumped to 1.2.1.
$ git commit -a -m "Bumped version number to 1.2.1"
[hotfix-1.2.1 41e61bb] Bumped version number to 1.2.1
1 files changed, 1 insertions(+), 1 deletions(-)

# 버그 해결 후 커밋
$ git commit -m "Fixed severe production problem"
[hotfix-1.2.1 abbe5d6] Fixed severe production problem
5 files changed, 32 insertions(+), 17 deletions(-)

# master 브랜치에 merge
$ git checkout master
Switched to branch 'master'
$ git merge --no-ff hotfix-1.2.1
Merge made by recursive.
(Summary of changes)
$ git tag -a 1.2.1

# develop 브랜치에 merge
$ git checkout develop
Switched to branch 'develop'
$ git merge --no-ff hotfix-1.2.1
erge made by recursive.
(Summary of changes)

# 브랜치 삭제
$ git branch -d hotfix-1.2.1
Deleted branch hotfix-1.2.1 (was abbe5d6).
 ```

### 개발 흐름

1. master 브랜치에서 시작한다.
2. master 브랜치에서 develop 브랜치를 시작한다. 이 브랜치에서 개발을 진행한다.
3. 개발을 진행하다가 회원가입, 장바구니 등의 기능 구현이 필요할 경우 A개발자는 develop 브랜치에서 feature 브랜치를 하나 생성해서 회원가입 기능을 구현하고, B개발자도 develop 브랜치에서 feature 브랜치를 하나 생성해서 장바구니 기능을 구현한다.
4. 완료된 feature 브랜치는 검토를 거쳐 다시 develop 브랜치에 병합(merge)한다.
5. 이제 모든 기능이 완료되면 develop 브랜치에서 release 브랜치를 만든다. 그리고 QA(품질검사)를 하면서 보완점을 보완하고 버그를 픽스한다.
6. 모든 것이 완료되면 이제 release 브랜치를 master 브랜치와 develop 브랜치에 병합(merge)한다. master 브랜치에서는 버전추가를 위해 태그를 하나 생성하고 배포한다.
7. 배포를 했는데 미처 발견하지 못한 버그가 있을 경우 hotfix 브랜치를 만들어 긴급 수정 후 태그를 생성하고 바로 수정 배포를 한다.

<br/>

> 🔖 참고. [A successful Git branching model](http://dogfeet.github.io/articles/2011/a-successful-git-branching-model.html)