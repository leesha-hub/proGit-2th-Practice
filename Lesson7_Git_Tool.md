7장 Git 도구 
일상적으로 사용하지는 않지만 위급한 상황에서 필요한 Git 도구</br>

7.1 리비전 조회하기</br>
-리비전 하나 가리키기</br>

-SHA-1 줄여 쓰기</br>
git show [SHA-1 OR SHA-1 앞 네글자]</br>
git --abbrev-commit : 짧고 중복되지 않는 해시값을 보여준다</br>
git log --abbrev-commit --pretty=oneline</br>

-브랜치로 가리키기</br>
git show [브랜치]</br>
git rev-parse [브랜치]</br>

-RefLog로 가리키기</br>
RefLog란? : Git은 자동으로 브랜치와 HEAD가 지난 몇 달 동안에 가리켰었던 커밋을 모두 기록하는데 이 로그를 Reflog라고 한다</br>
git show HEAD@{5}</br>
git show [브랜치]@{yesterday}</br>
git log -g master : git reflog 결과를 git log 명령과 같은 형태로 볼수 있다</br>

- 계통 관계로 가리키기</br>
git show HEAD^ :　HEAD^는 HEAD의 부모로 바로 이전의 커밋을 보여준다</br>
git show [SHA-1]^2 : 두 번째 부모. 첫 번째 부모는 checkout 브랜치를 뜻하고 두 번째 부모는 merge한 대상 브랜치를 의미한다</br>

- 범위로 커밋 가리키기</br>
(1)Double Dot : 어떤 커밋들이 한쪽에는 관련됐고 다른 쪽에는 관련되지 않았는지 Git에 물어보는 것</br>
git log master..experiment : master에는 없지만 experiment에는 있는 커밋</br>
git log experiment..master : experiment에는 없지만 master에는 있는 커밋</br>
git log origin/master..HEAD : master 브랜치에는 없고 현재 checkout 중인 브랜치에만 있는 커밋</br>

(2) 세 개 이상의 Refs</br>
Double Dot은 간단하고 유용하지만 두 개 이상의 브랜치에는 사용할수 없다.</br>
git log refA.refB</br>
git log ^refA refB</br>
git log refB --not refA</br>

(3) Triple Dot</br>
양쪽에 있는 두 Refs 사이에서 공통으로 가지는 것을 제외하고 서로 다른 커밋만을 보여준다.</br>
git log master...experiment REDC</br>

7.2 대화형 명령</br>
git add -i</br>

- Staging Area에 파일 추가하고 추가 취소하기</br>
what now>프롬포트에서 2나 u를(update) 입력하면 Staging Area에 추가할 수 있는 파일을 전부 보여준다 > Staging Area에 올려두고 rever도 가능</br>

- 파일 일부분만 Staging Area에 추가하기</br>
what now>프롬포트에서 5(patch) 입력한다. </br>
git add -p</br>
git add --patch</br>
git reset --patch </br>
git stash save --patch</br>

7.3 Stashing과 Cleaning</br>
git stash</br>
git stash list</br>
git stash apply</br>
git stash drop</br>
git stash pop</br>

-stash를 만드는 새로운 방법</br>
git stash --keep-index</br>
git stash -u</br>
git stash --patch</br>

-stash를 적용한 브랜치 만들기</br>
git stash branch testchanges</br>

-워킹디렉토리 청소하기</br>
git clean</br>
git stash -all</br>
git clean -f -d</br>

7.4 내 작업에 서명하기</br>
-GPG소개</br>
git --list-keys</br>
git --gen-key</br>
git config -global user.singingkey [SHA-1]</br>

-태그 서명하기</br>

-태그 확인하기</br>

-커밋에 서명하기</br>

7.5 검색</br>
- git grep</br>
git grep -n gmtime_r</br>
git grep --count gmtime_r</br>
git grep -p gmtime_r *.c01</br>

- git 로그 검색</br>
git log -SZLIB_BUF_MAX --oneline : 상수가에 대한 로그</br>

- 라인 로그 검색</br>
git log -L : git_deflate_bound:zlib.c</br>

7.6 히스토리 단장하기</br>
- 마지막 커밋을 수정하기</br>
git commit --amend</br>

- 커밋 메시지를 여러 개 수정하기</br>
rebase 명령어 사용</br>
git rebase -i HEAD~3</br>

- 커밋 순서 바꾸기</br>
대화형 Rebase 도구로 커밋 전체를 삭제하거나 순서를 조정할수 있음</br>

- 커밋 합치기</br>
pick이나 edit 말고 squash를 입력하면 git은 해당 커밋과 바로 이전 커밋을 합치고 커밋 메시지도 merge한다</br>

- 커밋 분리하기</br>
git rebase -i</br>
커밋을 edit으로 수정</br>
git reset HEAD^으로 커밋 해제</br>
git add README 수정한 파일 staged 상태로 변경</br>
git commit -m 'update README Only'</br>
git add lib/simplegit.rb</br>
git commit -m 'added blame'</br>
git rebase --continue</br>

-filter-branch는 포크레인</br>
수정해야 하는 커밋이 너무 많아서 rebase로는 불가능할 때</br>
git filter-branch --tree-filter 'rm -f passwords.txt' HEAD</br>
히스토리에서 특정 파일 모두 제거 가능</br>

- 하위 디렉터리를 루트 디렉터리로 만들기</br>
git filter-branch --subdirectory-filter trunk HEAD</br>

- 모든 커밋의 이메일 주소를 수정하기</br>
git filter-branch --commit-filter</br>

7.7 Reset 명확히 알고 가기</br>
git은 일반적으로 세 가지 트리를 관리하는 시스템이다</br>

트리</br>
역할</br>
HEAD</br>
마지막 커밋 스냅샷, 다음 커밋의 부모 커밋</br>
Index</br>
다음에 커밋할 스냅샷</br>
워킹 디렉토리</br>
샌드박스</br>

- HEAD</br>
현재 브랜치를 가리키는 포인터. 브랜치에 담긴 가장 마지막 커밋을 가리킨다. 마지막 커밋의 스냅샷이다.</br>
git cat-file -p HEAD 스냅샷의 드렉터리 리스팅</br>
git ls-tree -r HEAD : 각 파일의 SHA-1 체크섬을 보여줌</br>

- Index</br>
바로 다음에 커밋할 것들. staging Area에 올라와 있는 것. git commit 명령을 실행했을 때 git이 처리할 것들이 있는곳</br>

- 워킹 디렉터리</br>
커밋 전에는 Index(Staging Area)에 올려놓고 얼마든지 변경할 수 있다</br>

- 워크플로</br>

7.8 고급 Merge</br>
오랫동안 합치지 않은 두 브랜치를 한 번에 Merge하면 거대한 충돌이 발생한다. 조그마한 충돌을 자주 겪고 그걸 풀어나감으로써 브랜치를 최신으로 유지하는 것이 낫다</br>

- Merge충돌</br>
git merge whitespace</br>

- Merge 취소</br>
git merge --abort</br>
git reset --hard HEAD</br>
완전히 되돌리지 못하는 유일한 경우는 Merge 전의 워킹 디렉토리에서 Stash하지 않았거나 커밋하지 않은 파일이 있었을 경우</br>

- 공백 무시</br>
git merge -Xignore-space-all</br>
git merge -Xignore-space-change</br>
ex) git merge -Xignore-space-change whitespace</br>

- 수동으로 merge하기</br>
git show</br>
git ls-files -u</br>
git merge-file</br>
git diff --ours</br>
git diff --theirs</br>
git diff --base</br>

- 충돌파일 checkout</br>
git checkout --conflict=diff3 hello.rb</br>

- Merge 로그</br>
git log --oneline --left-right HEAD...MERGE_HEAD</br>
git log --oneline --left-right --merge</br>

- Combined Diff 형식</br>
git diff</br>
git log --cc -p -1</br>

- Merge 되돌리기</br>
Refs 수정 : 로컬에만 있을때는 브랜치를 원하는 커밋으로 옮긴다. 잘못 Merge하고 나서 git reset --hard HEAD~ 명령으로 브랜치를 되돌리면 된다.</br>
커밋 되돌리기(revert) : 브랜치를 옮기지 못할 경우 모든 변경사항을 취소하는 새로운 커밋을 만들 수 있다. </br>
git revert -m 1 HEAD</br>

- 다른 방식의 Merge</br>
Merge는 보통 recursive 전략을 사용한다. 브랜치를 한번에 Merge하는 방법은 여러 가지다</br>
Our/Their 선택하기 : 두 브랜치 중 한쪽을 선택하여 Merge</br>
git merge -Xours mundo</br>
git merge -Xtheirs mundo</br>
거짓 merge : our 브랜치 코드를 그대로 사용하고 Merge한 것처럼 기록한다</br>
git merge -s ours mundo</br>

- 서브트리 Merge</br>
프로젝트가 2개 있을 때 한 프로젝트를 다른 프로젝트의 하위 디렉토리로 매핑하여 사용하는 방법</br>
git read-tree --prefix=rack/ -u rack_branch</br>
git checkout rack_branch</br>
git pull</br>
git checkout master</br>
git merge --squash -s resursive -Xsubtree=rack rack_branch</br>
git diff-tree -p rack_branch</br>

7.9 Rerere</br>
“resue recorded resolution"은 기록한 해결책 재사용하기라는 뜻 </br>
git rerere status</br>
git rerere diff</br>

7.10 Git으로 버그 찾기</br>
git blame</br>
git blame -C </br>

- 이진탐색</br>
git bisect start</br>
git bisect bad</br>
git bisect good v1.0</br>
git bisect reset</br>

7.11 서브모듈</br>
프로젝트를 수행하다 보면 다른 프로젝트를 함께 사용해야 하는 경우가 있다. 두 프로젝트를 서로 별개로 다루면서 그중 하나를 다른 하나 안에서 사용할 수 있어야 한다.</br>

- 서브모듈 시작하기</br>
git submodule add [저장소 주소]</br>
git config submodule.DbConnector.url PRIVATE_URL</br>
git diff --cached DbConnector : DbConnector 디렉터리를 서브모듈로 취급하기 때문에 해당 디렉터리 아래의 파일 수정사항을 직접 추적하지 않는다. 대신 통째로 특별한 커밋으로 취급한다.</br>
git diff --cached --submodule</br>

- 서브모듈 포함한 프로젝트 Clone</br>
기본적으로 서브모듈 디렉터리는 빈 디렉터리다</br>
git submodule init</br>
git submodule update</br>
git clone -recursive [저장소 주소]</br>

- 서브모듈 포함한 프로젝트 작업</br>
서브모듈 업데이트</br>
서브모듈 디렉터리에서 git fetch > git merge</br>
git submodule update --remote DbConnector</br>
git config.submodulesummary 서브모듈의 변경사항을 간단히 보여줌</br>
git log -p --submodule</br>

- 서브모듈 관리하기</br>
서브모듈 디렉터리로 가서 브랜치를 Checkout</br>
git checkout stable</br>
git submodule update --remote --merge</br>

- 서브모듈 수정 사항 공유하기</br>
서브모듈의 변경사항을 Push하지 않은 채로 메인 프로젝트에서 커밋을 Push하면 안된다. 이런 불상사가 발생하지 않도록 Push 했는지 검사하도록 Git에 물어본다</br>
git push --recurse-submodules=check</br>
git push --recurse-submodules=on-demand : Git이 메인 프로젝트를 Push하기 전에 DbConnector 모듈로 들어가서 Push한다</br>

- 서브모듈 Merge하기</br>
1. 먼저 충돌을 해결한다.</br>
2. 메인 프로젝트로 돌아간다.</br>
3. SHA-1을 검사한다</br>
4. 충돌난 서브모듈을 해결한다</br>
5. Merge 결과를 커밋한다</br>

- 서브모듈 팁</br>
Foreach</br>
git submodule foreach 'git stash'</br>
git submodule foreach 'git checkout -b featureA'</br>
git submodule foreach 'git diff'</br>

- 유용한 Alias</br>

- 서브모듈을 사용할 때 주의할 점들</br>
서브모듈의 코드를 수정하는 경우에 주의를 요한다</br>

7.12 Bundle</br>
데이터를 한 파일에 몰아넣는 것</br>
git bundle create repo.bundle HEAD master</br>
git clone repo.bundle repo </br>
git bundle verify ../commits,bundle</br>
git bundle list-head ../commits.bundle</br>
git fetch ../commits.bundle master:other-master</br>

7.13 Replace</br>
저장한 Git의 개체는 기본적으로 변경할 수 없다. 하지만 변경된 것처럼 보이게는 가능하다</br>
1) 전체 히스토리 유지</br>
git branch history [SHA-1]</br>
git remote add project-history [저장소 주소]</br>
git push project-history history:master</br>
2) 최신 커밋 몇 개만 유지</br>
git commit-tree [SHA-1]^{tree}</br>
git rebase --onto [commit-tree SHA-1] [SHA-1]</br>

7.14 Credential 저장소</br>
SSH 프로토콜을 사용하여 리모트 저장소에 접근할 때 Passphase 없이 생성한 SSH Key를 사용하면 사용자의 이름과 비밀번호를 입력하지 않고도 안전하게 데이터를 주고받을 수 있다. 반면 HTTP 프로토콜을 사용하는 경우는 매번 사용자 이름과 비밀번호를 입력해야 한다.

Git은 인증정보(Credential)를 입력하는 경우 인증정보를 저장해두고 자동으로 입력해주는 시스템을 제공한다.
