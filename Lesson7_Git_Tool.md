7장 Git 도구 
일상적으로 사용하지는 않지만 위급한 상황에서 필요한 Git 도구

7.1 리비전 조회하기
-리비전 하나 가리키기

-SHA-1 줄여 쓰기
git show [SHA-1 OR SHA-1 앞 네글자]
git --abbrev-commit : 짧고 중복되지 않는 해시값을 보여준다
git log --abbrev-commit --pretty=oneline

-브랜치로 가리키기
git show [브랜치]
git rev-parse [브랜치]

-RefLog로 가리키기
RefLog란? : Git은 자동으로 브랜치와 HEAD가 지난 몇 달 동안에 가리켰었던 커밋을 모두 기록하는데 이 로그를 Reflog라고 한다
git show HEAD@{5}
git show [브랜치]@{yesterday}
git log -g master : git reflog 결과를 git log 명령과 같은 형태로 볼수 있다

- 계통 관계로 가리키기
git show HEAD^ :　HEAD^는 HEAD의 부모로 바로 이전의 커밋을 보여준다
git show [SHA-1]^2 : 두 번째 부모. 첫 번째 부모는 checkout 브랜치를 뜻하고 두 번째 부모는 merge한 대상 브랜치를 의미한다

- 범위로 커밋 가리키기
(1)Double Dot : 어떤 커밋들이 한쪽에는 관련됐고 다른 쪽에는 관련되지 않았는지 Git에 물어보는 것
git log master..experiment : master에는 없지만 experiment에는 있는 커밋
git log experiment..master : experiment에는 없지만 master에는 있는 커밋
git log origin/master..HEAD : master 브랜치에는 없고 현재 checkout 중인 브랜치에만 있는 커밋

(2) 세 개 이상의 Refs
Double Dot은 간단하고 유용하지만 두 개 이상의 브랜치에는 사용할수 없다.
git log refA.refB
git log ^refA refB
git log refB --not refA

(3) Triple Dot
양쪽에 있는 두 Refs 사이에서 공통으로 가지는 것을 제외하고 서로 다른 커밋만을 보여준다.
git log master...experiment REDC

7.2 대화형 명령
git add -i

- Staging Area에 파일 추가하고 추가 취소하기
what now>프롬포트에서 2나 u를(update) 입력하면 Staging Area에 추가할 수 있는 파일을 전부 보여준다 > Staging Area에 올려두고 rever도 가능

- 파일 일부분만 Staging Area에 추가하기
what now>프롬포트에서 5(patch) 입력한다. 
git add -p
git add --patch
git reset --patch 
git stash save --patch

7.3 Stashing과 Cleaning
git stash
git stash list
git stash apply
git stash drop
git stash pop

-stash를 만드는 새로운 방법
git stash --keep-index
git stash -u
git stash --patch

-stash를 적용한 브랜치 만들기
git stash branch testchanges

-워킹디렉토리 청소하기
git clean
git stash -all
git clean -f -d

7.4 내 작업에 서명하기
-GPG소개
git --list-keys
git --gen-key
git config -global user.singingkey [SHA-1]

-태그 서명하기

-태그 확인하기

-커밋에 서명하기

7.5 검색
- git grep
git grep -n gmtime_r
git grep --count gmtime_r
git grep -p gmtime_r *.c01

- git 로그 검색
git log -SZLIB_BUF_MAX --oneline : 상수가에 대한 로그

- 라인 로그 검색
git log -L : git_deflate_bound:zlib.c

7.6 히스토리 단장하기
- 마지막 커밋을 수정하기
git commit --amend

- 커밋 메시지를 여러 개 수정하기
rebase 명령어 사용
git rebase -i HEAD~3

- 커밋 순서 바꾸기
대화형 Rebase 도구로 커밋 전체를 삭제하거나 순서를 조정할수 있음

- 커밋 합치기
pick이나 edit 말고 squash를 입력하면 git은 해당 커밋과 바로 이전 커밋을 합치고 커밋 메시지도 merge한다

- 커밋 분리하기
git rebase -i
커밋을 edit으로 수정
git reset HEAD^으로 커밋 해제
git add README 수정한 파일 staged 상태로 변경
git commit -m 'update README Only'
git add lib/simplegit.rb
git commit -m 'added blame'
git rebase --continue

-filter-branch는 포크레인
수정해야 하는 커밋이 너무 많아서 rebase로는 불가능할 때
git filter-branch --tree-filter 'rm -f passwords.txt' HEAD
히스토리에서 특정 파일 모두 제거 가능

- 하위 디렉터리를 루트 디렉터리로 만들기
git filter-branch --subdirectory-filter trunk HEAD

- 모든 커밋의 이메일 주소를 수정하기
git filter-branch --commit-filter

7.7 Reset 명확히 알고 가기
git은 일반적으로 세 가지 트리를 관리하는 시스템이다

트리
역할
HEAD
마지막 커밋 스냅샷, 다음 커밋의 부모 커밋
Index
다음에 커밋할 스냅샷
워킹 디렉토리
샌드박스


- HEAD
현재 브랜치를 가리키는 포인터. 브랜치에 담긴 가장 마지막 커밋을 가리킨다. 마지막 커밋의 스냅샷이다.
git cat-file -p HEAD 스냅샷의 드렉터리 리스팅
git ls-tree -r HEAD : 각 파일의 SHA-1 체크섬을 보여줌

- Index
바로 다음에 커밋할 것들. staging Area에 올라와 있는 것. git commit 명령을 실행했을 때 git이 처리할 것들이 있는곳

- 워킹 디렉터리
커밋 전에는 Index(Staging Area)에 올려놓고 얼마든지 변경할 수 있다

- 워크플로

7.8 고급 Merge
오랫동안 합치지 않은 두 브랜치를 한 번에 Merge하면 거대한 충돌이 발생한다. 조그마한 충돌을 자주 겪고 그걸 풀어나감으로써 브랜치를 최신으로 유지하는 것이 낫다

- Merge충돌
git merge whitespace

- Merge 취소
git merge --abort
git reset --hard HEAD
완전히 되돌리지 못하는 유일한 경우는 Merge 전의 워킹 디렉토리에서 Stash하지 않았거나 커밋하지 않은 파일이 있었을 경우

- 공백 무시
git merge -Xignore-space-all
git merge -Xignore-space-change
ex) git merge -Xignore-space-change whitespace

- 수동으로 merge하기
git show
git ls-files -u
git merge-file
git diff --ours
git diff --theirs
git diff --base

- 충돌파일 checkout
git checkout --conflict=diff3 hello.rb

- Merge 로그
git log --oneline --left-right HEAD...MERGE_HEAD
git log --oneline --left-right --merge

- Combined Diff 형식
git diff
git log --cc -p -1

- Merge 되돌리기
Refs 수정 : 로컬에만 있을때는 브랜치를 원하는 커밋으로 옮긴다. 잘못 Merge하고 나서 git reset --hard HEAD~ 명령으로 브랜치를 되돌리면 된다.
커밋 되돌리기(revert) : 브랜치를 옮기지 못할 경우 모든 변경사항을 취소하는 새로운 커밋을 만들 수 있다. 
git revert -m 1 HEAD

- 다른 방식의 Merge
Merge는 보통 recursive 전략을 사용한다. 브랜치를 한번에 Merge하는 방법은 여러 가지다
Our/Their 선택하기 : 두 브랜치 중 한쪽을 선택하여 Merge
git merge -Xours mundo
git merge -Xtheirs mundo
거짓 merge : our 브랜치 코드를 그대로 사용하고 Merge한 것처럼 기록한다
git merge -s ours mundo

- 서브트리 Merge
프로젝트가 2개 있을 때 한 프로젝트를 다른 프로젝트의 하위 디렉토리로 매핑하여 사용하는 방법
git read-tree --prefix=rack/ -u rack_branch
git checkout rack_branch
git pull
git checkout master
git merge --squash -s resursive -Xsubtree=rack rack_branch
git diff-tree -p rack_branch

7.9 Rerere
“resue recorded resolution"은 기록한 해결책 재사용하기라는 뜻 
git rerere status
git rerere diff

7.10 Git으로 버그 찾기
git blame
git blame -C 

- 이진탐색
git bisect start
git bisect bad
git bisect good v1.0
git bisect reset

7.11 서브모듈
프로젝트를 수행하다 보면 다른 프로젝트를 함께 사용해야 하는 경우가 있다. 두 프로젝트를 서로 별개로 다루면서 그중 하나를 다른 하나 안에서 사용할 수 있어야 한다.

- 서브모듈 시작하기
git submodule add [저장소 주소]
git config submodule.DbConnector.url PRIVATE_URL
git diff --cached DbConnector : DbConnector 디렉터리를 서브모듈로 취급하기 때문에 해당 디렉터리 아래의 파일 수정사항을 직접 추적하지 않는다. 대신 통째로 특별한 커밋으로 취급한다.
git diff --cached --submodule

- 서브모듈 포함한 프로젝트 Clone
기본적으로 서브모듈 디렉터리는 빈 디렉터리다
git submodule init
git submodule update
git clone -recursive [저장소 주소]

- 서브모듈 포함한 프로젝트 작업
서브모듈 업데이트
서브모듈 디렉터리에서 git fetch > git merge
git submodule update --remote DbConnector
git config.submodulesummary 서브모듈의 변경사항을 간단히 보여줌
git log -p --submodule

- 서브모듈 관리하기
서브모듈 디렉터리로 가서 브랜치를 Checkout
git checkout stable
git submodule update --remote --merge

- 서브모듈 수정 사항 공유하기
서브모듈의 변경사항을 Push하지 않은 채로 메인 프로젝트에서 커밋을 Push하면 안된다. 이런 불상사가 발생하지 않도록 Push 했는지 검사하도록 Git에 물어본다
git push --recurse-submodules=check
git push --recurse-submodules=on-demand : Git이 메인 프로젝트를 Push하기 전에 DbConnector 모듈로 들어가서 Push한다

- 서브모듈 Merge하기
1. 먼저 충돌을 해결한다.
2. 메인 프로젝트로 돌아간다.
3. SHA-1을 검사한다
4. 충돌난 서브모듈을 해결한다
5. Merge 결과를 커밋한다

- 서브모듈 팁
Foreach
git submodule foreach 'git stash'
git submodule foreach 'git checkout -b featureA'
git submodule foreach 'git diff'

- 유용한 Alias

- 서브모듈을 사용할 때 주의할 점들
서브모듈의 코드를 수정하는 경우에 주의를 요한다

7.12 Bundle
데이터를 한 파일에 몰아넣는 것
git bundle create repo.bundle HEAD master
git clone repo.bundle repo 
git bundle verify ../commits,bundle
git bundle list-head ../commits.bundle
git fetch ../commits.bundle master:other-master

7.13 Replace
저장한 Git의 개체는 기본적으로 변경할 수 없다. 하지만 변경된 것처럼 보이게는 가능하다.
1) 전체 히스토리 유지
git branch history [SHA-1]
git remote add project-history [저장소 주소]
git push project-history history:master
2) 최신 커밋 몇 개만 유지
git commit-tree [SHA-1]^{tree}
git rebase --onto [commit-tree SHA-1] [SHA-1]

7.14 Credential 저장소
SSH 프로토콜을 사용하여 리모트 저장소에 접근할 때 Passphase 없이 생성한 SSH Key를 사용하면 사용자의 이름과 비밀번호를 입력하지 않고도 안전하게 데이터를 주고받을 수 있다. 반면 HTTP 프로토콜을 사용하는 경우는 매번 사용자 이름과 비밀번호를 입력해야 한다.

Git은 인증정보(Credential)를 입력하는 경우 인증정보를 저장해두고 자동으로 입력해주는 시스템을 제공한다.