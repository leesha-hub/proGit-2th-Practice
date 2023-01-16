ProGit 1장. 버전관리란?

1.1 버전관리란?</br>
버전관리시스템은 파일 변화를 시간에 따라 기록했다가 나중에 특정시점의 버전을 다시 꺼내올수 있는 시스템을 말한다</br>
⁃ 로컬 버전 관리(Version Control): 디렉터리로 파일을 복사하는 방법</br>
⁃ 중앙집중식 버전 관리(Center Version Control) : 파일을 관리하는 서버가 별도로 존재하고 클라이언트가 중앙에서 파일을 다운받아 사용</br>
⁃ 분산 버전 관리 시스템(Divide Version Control) : 저장소를 전부 복제하는 방식. Checkout은 모든 데이터를 가진 진정한 백업. 리모트 저장소가 존재하여 협업 가능</br> 

1.2 짧게 보는 Git의 역사</br>
리눅스 커널 프로젝트에서 사용하던 BitKeeper</br>
⁃ 빠른 속도</br>
⁃ 단순한 구조</br>
⁃ 비선형적인 개발(수천 개의 동시다발적인 브랜치)</br>
⁃ 완벽한 분산</br>
⁃ 리눅스 커널 같은 대형 프로젝트에도 유용할 것(속도나 데이터 크기 면에서)</br>

1.3 Git의 기초</br>
⁃ 차이가 아니라 스냅샷. 깃은 데이터를 스냅샷의 스트림처럼 취급한다</br>
⁃ 거의 모든 명령을 로컬에서 실행</br>
⁃ Git의 무결성 : 체크섬 없이는 어떠한 파일이나 디렉터리도 변경할 수 없다. </br>
⁃ Git은 데이터를 추가할뿐 </br>
⁃ 세가지 상태 : Committed, Modified, Staged </br>

1.4 CLI(Command Line Interface) </br>

1.5 Git 설치</br>

1.6 Git 최초 설정</br>
⁃ /etc/gitconfig 파일 : 시스템의 모든 사용자와 모든 저장소에 적용되는 설정이다. git config --system 옵션으로 이 파일을 읽고 쓸 수 있다.</br>
⁃ ~/.gitconfig, ~/.config/git/config 파일 : 특정 사용자에게만 적용되는 설정이다. git config --global 옵션으로 이 파일을 읽고 쓸 수 있다.</br>
⁃ .git/config 파일 : Git 디렉터리에 있고 특정 저장소(혹은 현재 작업 중인 프로젝트)에만 적용된다.</br>

⁃ 사용자 정보</br>
git config --global user.name "[이름]"</br>
git config --global user.email "[이메일]"</br>

⁃ 편집기</br></br>

⁃ 설정 확인</br>
git config --list</br>

1.7 도움말 보기</br>
git help <verb></br>
git <verb> --help</br>
man git-<verb></br>
git help config</br>
