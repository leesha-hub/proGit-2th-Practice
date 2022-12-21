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
