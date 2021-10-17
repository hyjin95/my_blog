# cmd, git, terminal 명령어

## git

### pwd

* 현재위치

### mkdir

* mkdir 파일명\
  \- 디렉토리(폴더) 생성

### cd

* change directory
* cd 파일명\
  \- 해당 디렉토리로 들어가기\
  \- 폴더를 드래그해 cmd로 가져오면 자동으로 경로를 가져온다.
* cd..\
  \- 상위 디렉토리로 돌아가기

### git init

* 현재 디렉토리를 git (버전) 저장소로 지정

### ls -al

* 숨김 폴더, 숨김 파일 확인하기

### git

* 사용할 수 있는 명령어 확인하기

### git config

* 설정 명령어
* git config --global user.name\
  git config --global user.email
* Git을 설치하고 나서 가장 먼저 해야 하는 것은 사용자이름과 이메일 주소를 설정하는 것이다. Git은 커밋할 때마다 이 정보를 사용한다. 한 번 커밋한 후에는 정보를 변경할 수 없다.
* git config --global core.editor "vim"
* 간혹 기본 에디터가 vim이 아닌 경우 작성해 에디터를 변경해준다.

### git init

* 버전 관리(형상 관리)할 폴더 지정

### git status

* 해당 디렉토리의 형상관리 상태 확인
* 등록된 파일, 미등록된 파일 등을 확인할 수 있다.

### git add 파일명

* 해당 파일을 버전관리하기위해 git이 추적할 수 있도록 등록한다.
* 프로젝트에서는 테스트 등을 이유로 일회성 문서를 만들어 사용하는 경우가 있는데 그런 문서들은 버전관리하지 않으므로 해당 기능을 사용해 관리할 파일을 지정해줘야한다.
* 파일이 수정되어 새 버전을 만들때에도 반드시 add를 해야한다.

### git log

* git 로그를 확인할 수 있다. \
  \- 생성한 버전 등

## vim

### vim

* vim 에디터 사용 시작

### i

* insert(입력) 모드 전환

### esc

* vim 에디터 나가기, 명령어 입력 가능

### esc > :q! > return

* 변경사항 없이 vim 에디터 종료

### esc > :wq > return

* 변경사항 저장 후 vim 에디터 종료
