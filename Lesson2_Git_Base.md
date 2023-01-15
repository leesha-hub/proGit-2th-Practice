ProGit 2장 Git의 기초

2장에서 다루는 내용</br>
⁃ 저장소를 만들고 설정하는 법</br>
⁃ 파일을 추적(Track)하거나 추적을 그만두는 방법</br>
⁃ 변경 내용을 Stage하고 커밋하는 방법</br>
⁃ 파일이나 파일 패턴을 무시하도록 Git을 설정하는 방법</br>
⁃ 실수를 쉽고 빠르게 만회하는 방법</br>
⁃ 프로젝트 히스토리를 조회하고 커밋을 비교하는 방법</br>
⁃ 리모트 저장소에 Push하고 Pull하는 방법</br>

2.1 저장소 만들기</br>
⁃ 방법1 : 기존 프로젝트나 디렉터리를 Git저장소로 만드는 방법</br>
⁃ 방법2 : git clone</br>

2.2 수정하고 저장소에 저장하기</br>
⁃ Track파일 : 이미 스냅샷에 포함돼 있던 파일</br>
  ㄴ Unmodified & Modified & Staged</br>
⁃ Untracked 파일 : 스냅샷에도 Staging Area에도 포함되어 있지 않은 파일</br>

*스냅샷 : 변경된 파일 전체를 저장하지 않고 파일에서 변경된 부분을 찾아 수정된 내용만 저장. 마치 변화된 부분만 찾아 사진을 찍는 것 같다고 하여 스냅샷 방식이라고 한다</br>

파일의 라이프 사이클</br>

파일의 상태 확인하기</br>
git status</br>

파일을 새로 추적하기</br>
git add</br>

Modified 상태의 파일을 Stage 하기</br>
git add </br>
파일을 추적할때도 사용하고 수정한 파일을 Staged 상태로 만들 때도 사용된다</br>
Merge할때 충돌난 파일을 Resolve 상태로 만들 때도 사용한다</br>

파일 상태를 짤막하게 확인하기</br>
git status -s</br>
git status --short</br>

파일 무시하기</br>
.gitignore 파일 생성하여 무시할 파일 패턴 기입</br>

Stage와 Unstaged 상태의 변경 내용 보기</br>
git diff </br>
명령을 실행하면 수정했지만 아직 staged 상태가 아닌 파일을 비교해 볼 수 있다</br>
git diff --stage</br>
git diff --cached</br>

변경사항 커밋하기</br>
git commit -v</br>
git commit -m</br>
git은 커밋할때마다 스냅샷을 기록함. 나중에 스냅샷끼리 비교하거나 예전 스냅샷으로 되돌릴수 있다</br>

파일 삭제하기</br>
git rm</br>
Tracked 상태의 파일을 삭제한 후에(정확하게는 Staging Area에서 삭제) 커밋해야 함.</br>
실제 워킹 디렉터리에 있는 파일도 삭제됨</br>

파일 이름 변경하기</br>
파일 이름의 변경이나 파일 이동을 명시적으로 관리하지 않음</br>
git mv</br>
ㄴ git rm + git add</br>

2.3 커밋 히스토리 조회하기</br>
git log</br>
git log -p -2</br>
git log --stat</br>
git log -pretty=online</br>
git log --pretty=format:"~~~" -- graph</br>

git log -pretty=format에 쓸 수 있는 몇 가지 유용한 옵션</br>

*저자와 커미터:저자는 원래 작업을 수행한 원작자이고 커미터는 마지막으로 이 작업을 적용한 (저장소에 포함한) 사람</br>

git log 주요 옵션</br>

조회 제한조건</br>
git log --since=2.weeks</br>

git log조회 범위를 제한하는 옵션</br>

2.4 되돌리기</br>
git commit -amend</br>
ㄴ git commit -m 'initial commit'  </br>
git add forgotton file </br>
git commit -amend</br>

파일 상태를 Unstage로 변경하기</br>
git reset HEAD <file></br>
실수로 git add *을 실행할 경우, 파일이 모두 Staging 영역에 들어오게 된다</br>
--hard옵션과 함께 사용하면 워킹 디렉터리 파일까지 수정되기 때문에 조심해야 한다</br>

Modified 파일 되돌리기</br>
git checkout -- [file]</br>

2.5 리모트 저장소</br>
인터넷이나 네트워크 어딘가에 있는 저장소</br>
읽고 쓰기가 모두 가능한 저장소가 있을수 있고 읽기만 가능한 저장소도 있다</br>

리모트 저장소 확인하기</br>
git remote</br>
git remote -v</br>

리모트 저장소 추가하기</br>
git remote add [단축이름] [url]</br>

리모트 저장소를 Pull하거나 Fetch하기</br>
git fetch [remote-name]</br>
리모트 저장소에 있는 데이터를 모두 가져온다 </br>

*fetch는 원격 저장소에 변경사항이 있는지 확인만 하고 변경된 데이터를 로컬로 가져오지는 않는다. 반명 pull은 원격 저장소에서 변경된 메타데이터 정보를 확인할뿐만 아니라 최신 데이터를 복사하여 로컬 git에 가져온다.</br>

리모트 저장소에 Push하기</br>
git push origin master </br>

리모트 저장소 살펴보기</br>
git remote show</br>

리모트 저장소 이름을 바꾸거나 리모트 저장소를 삭제하기</br>
git remote rename pb paul</br>
git remote rm paul</br>

2.6 태그</br>
태그 조회하기</br>
git tag</br>

태그 붙이기</br>
git의 태그는 Leightweight와 Annotated로 두 종류가 있다.</br>
Leightweight : 단순한 특정 커밋에 대한 포인터</br>
Annotated : 태그를 만든 사람, 이메일, 태그를 만든 날짜, 태그 메시지등 정보 저장이 가능</br>

Annotated태그</br>
git tag -a v1.4 -m 'my version 1.4'</br>
git show v1.4</br>

Leightweight태그</br>
git tag v1.4-lw => 단순 커밋 정보만 볼수 있음</br>
git tag</br>

특정 커밋만 나중에 태그</br>
git tag -a v1.2 [checksum]</br>

태그 공유하기</br>
git push origin v1.5</br>
git push origin --tags</br>

태그를 Checkout 하기</br>
git checkout -b version2 v2.0.0</br>
태그는 브랜치와 달리 가리키는 커밋을 바꿀수 없는 이름이기 때문에 Checkout을 사용할수 없다.</br>
새로운 브랜치를 생성해야 한다</br>

2.7 git Alias</br>
git config --global alias.co checkout</br>
git config --global alias.br branch</br>
git config --global alias.ci commit</br>
git config --global alias.st status</br>
