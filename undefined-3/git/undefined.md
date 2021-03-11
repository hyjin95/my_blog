# 설치 및 실행

### git

* [https://git-scm.com/](https://git-scm.com/)
* git-bash 에서 git 입력 &gt; 설치확인

### 1. git-bash 실행 &gt; 형상관리 디렉토리 만들기

![](../../.gitbook/assets/1%20%28124%29.png)

* git 디렉토리를 만들 폴더로 경로 이동 &gt; cd 폴더이름 &gt; mkdir 디렉토리이름 &gt; ls -al 숨김파일 확인
* 디렉토리 위치에서 git init - 현재 디렉토리에서 작업을 진행하겠다고 git에 알리는 작업
* ls -al : .git 디렉토리가 생성된 것을 알 수 있다. - 버전관리에 대해 생성된 정보가 저장되는 곳 \(버전 정보\)

### 2. vim 에디터로 txt파일 생성하기

![](../../.gitbook/assets/2%20%2899%29.png)

* 버전관리 디렉토리 &gt; vim f1.txt - vim에디터를 사용해 f1이라는 txt 문서 생성 - 바로 입력하면 입력되지 않는다. &gt; 입력모드가 아니므로
* i 입력시 insert모드로 전환된다. - 문자, 코드 입력이 가능하다.
* esc 입력시 입력모드 종료 - 명령어 입력이 가능하다.
* :wq - 저장 및 vim 에디터 종료
*  ls -al  - 생성된 파일 확인

![](../../.gitbook/assets/3%20%2875%29.png)

* cat f1.txt - 파일 내용 보기 

### 3.버전관리하기 - 깃에 등록

![](../../.gitbook/assets/4%20%2853%29.png)

* git 디렉토리에서 git status 명령어를 작성하면 - f1.txt라는 파일은 추적되고 있지 않다.
* f1.txt는 버전관리가 되고있는 gittest파일 안에 있지만 git에게 버전관리를 할 파일이라고 알리지 않았기때문에 해당문서는 git 에게서 무시된다. 

![](../../.gitbook/assets/5%20%2838%29.png)

* git add 파일명 - 해당 파일을 git에게 추가한다. &gt; 버전관리 하겠다. - git이 해당 파일을 추적하도록 한다. - 프로젝트 진행시 임시로 테스트를 위해 일회성으로 파일을 만드는 경우가 있기때문에 해당 명령어로 git에 추가할 파일을 명시해야한다.
* git status로 확인해보면 f1.txt가 master 브랜치에 새로 등록된것을 확인할 수 있다.

### 4.버전관리하기 - 이름, 이메일 등록하기

![](../../.gitbook/assets/1%20%28126%29.png)

* 디렉토리 최초 설정시 한번만 등록하면된다.
* git에게 버전관리하는 파일에 대한 내 이름과 이메일을 등록한다. 외부에서 어떤사람이 작업했는지 알 수 있다. 

### 5. 버전관리하기 - 최초의 버전관리 git commit

![](../../.gitbook/assets/2%20%28102%29.png)

* git commit 명령어를 입력하게되면 vim이 실행된다. - 커밋 정보를 알려준다. - i를 눌러 입력모드로 전환한다.
* 현재 버전의 정보를 직접 작성한다.  - version\(commit\) message

![](../../.gitbook/assets/3%20%2878%29.png)

* 상단에 버전정보를 입력하고 esc &gt; :wq 로 저장하고 vim을 종료한다.

![](../../.gitbook/assets/4%20%2855%29.png)

* f1.txt 라는 파일이 새로운 버전이 되었다.

### 6. 버전 확인하기

![](../../.gitbook/assets/1%20%28127%29.png)

* git log 명령어를 통해 확인해보면  1 이라는 버전 메세지가 들어있는 버전이 생성되어있으며 버전 작성자의 이름과 이메일, 생성날짜를 확인할 수 있다.

### 7. 같은 파일 버전관리 반복해보기

![](../../.gitbook/assets/2%20%28101%29.png)

* ls -al 명령어로 파일을 확인하고 f1.txt파일을 vim으로 다시 연다.
* i &gt; 내용수정 &gt; esc &gt; :wq &gt; enter

![](../../.gitbook/assets/3%20%2876%29.png)

* git status 명령어로 확인해보면 f1.txt 파일이 붉은글씨로 수정된 상태임을 볼 수 있다.

![](../../.gitbook/assets/4%20%2854%29.png)

* 바로 git commit을 하지않고 다시 버전관리 시스템에게 add해야 한다.

![](../../.gitbook/assets/1%20%28125%29.png)

* git add 파일명
* git status 명령어로 정상적으로 add된 것을 볼 수 있다.
* git commit을 진행한다.

![](../../.gitbook/assets/2%20%28100%29.png)

* i &gt; 버전 정보 작성 &gt; esc &gt; :wq &gt; enter
* git log 명령어로 확인한다.

![](../../.gitbook/assets/3%20%2877%29.png)

* log에서 버전2가 커밋된 것을 확인할 수 있다.

