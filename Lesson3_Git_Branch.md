3장 Git 브랜치

3.1 브랜치란 무엇인가?</br>
커밋 사이를 가볍게 이동할 수 있는 포인터!</br>

- 새 브랜치 생성하기</br>
git branch [이름] </br>
새로 만든 브랜치도 지금 작업하고 있는 마지막 커밋을 가리킨다. </br>
git은 HEAD라는 특수한 포인터가 있다.</br>

브랜치가 어떤 커밋을 가리키는지 확인할 수 있다</br>
*git log --decorate</br>
*git log --online --decorate --graph --all</br>

- 브랜치 이동하기</br>
git checkout [이름]</br>
checkout을 사용하면 HEAD는 [이름]브랜치를 가리킨다.</br>

3.2 브랜치와 Merge의 기초</br>

- 브랜치의 기초</br>
git checkout -b iss53 : git branch [이름] + git checkout [이름]</br>
git commit -a -m [메시지]</br>
git checkout master</br>
git checkout -b hotfix</br>
git commit -a -m [메시지]</br>
git checkout master</br>
git merge hotfix : Merge할 브랜치가 가리키는 커밋이 현 브랜치 커밋의 Upstream 브랜치기 때문에 master 브랜치 포인터는 Merge 과정 없이 최신 커밋으로 이동한다. 이런 Merge 방식을 Fastforward라고 부른다</br>
git branch -b hotfix</br>
git checkout iss53</br>
git commit -a -m [메시지] : git merge master 명령을 사용하면 iss53 브랜치에 hotfix가 적용된다. master 브랜치에서 git merge iss53을 하는 경우에도 hotfix와 iss53이 합쳐진다</br>

- Merge의 기초</br>
git checkout master</br>
git merge iss53 : hotfix를 merge했을 때와 다르다. 현재 브랜치가 가리키는 커밋이 Merge할 브랜치의 조상이 아니므로 git은 각 브랜치가 가리키는 커밋 두 개와 조상 하나를 사용하여 3-way Merge를 한다.</br>

3-way Merge 결과를 별도의 커밋으로 만들고 master가 그 커밋을 가리키도록 이동시킨다. 이런 커밋을 부모가 여러 개인 Merge 커밋이라 부른다</br>

git branch -d iss53</br>

- 충돌의 기초</br>
가끔 3-way Merge가 실패할 때도 있다. iss53과 hotfix가 같은 부분을 수정했다면 git은 Merge하지 못하고 충돌을 발생시킨다.</br>

3.3 브랜치 관리</br>
git branch</br>
브랜치의 목록을 보여준다</br>
git branch -v</br>
브랜치마다 마지막 커밋 메시지를 함께 보여준다</br>
git branch --merge</br>
git branch --no-merge</br>

3.4 브랜치 워크플로</br>
Long-Running 브랜치</br>
배포했거나 배포할 코드만 master 브랜치에 Merge하여 안정 버전의 코드만 master 브랜치에 둔다. 규모가 크고 복잡한 프로젝트일수록 유용하다.</br>
토픽 브랜치</br>
프로젝트 크기에 상관없이 유용하다. 어떤 한 가지 주제나 작업을 위해 만든 짧은 호흡의 브랜치다.</br>

3,5 리모트 브랜치</br>
리모트 Refs는 리모트 저장소에 있는 포인터인 레퍼런스</br>
git ls-remote : 모든 리모트 Refs를 조회할 수 있다</br>
git remote show : 모든 리모트 브랜치와 그 정보를 보여준다</br>

3.6 Rebase 하기</br>
