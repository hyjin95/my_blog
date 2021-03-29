---
description: 2021.03.29 - 월요일
---

# 리액트 개발환경

## 개발환경

### 1. npm 설치 및 확인

![](../../../.gitbook/assets/1%20%28133%29.png)

* Node js에서 LTS 안정화된 버전을 설치한다.
* CMD / 터미널에서 'node -v' 명령어로 버전을 확인해 설치여부를 알 수 있다. 마찬가지로 'npm -v'명령어로 npm 설치여부를 확인한다.
* 'create-react-app -V' 명령어로 설치확인 \(V는 대문자\)
* 더보기 - 앱설치 1, 2 - error : EACCES

### 2. 디렉토리 rect개발환경 설정

![](../../../.gitbook/assets/2%20%28108%29.png)

* 사용할 디렉토리\(폴더\)생성 &gt; react 개발환경 세팅하기 디렉토리에는 환경설정이 저장된다. create-react-app을 사용해 디렉토리를  react 환경으로 설정 한다.
* 'cd 디렉토리' 만들어둔 디렉토리 경로로 이동한다.
* 'create-react-app .' 해당 디렉토리가 create-react-app에 의해 react 환경으로 세팅된다.

![](../../../.gitbook/assets/3%20%2882%29.png)

### 3. VSCode로 실행 해보기

* 해당 디렉토리가 react로 설정이 되어 폴더를 터미널에서 실행시켜보면  rect기본 앱을 확인할 수 있다.
* 'npm run start' terminal에 입력해 npm으로 서버를 시작한다.
* ctrl + c &gt; 'y' 서버를 종료한다. &gt; 확인

![](../../../.gitbook/assets/2%20%28109%29.png)

* ip, port번호로 작성된 주소를 터미널에 보여준다. 해당 주소 중 하나로 브라우저에서 확인할 수 있다.
* Local : 로컬 주소 Your Network : 공유 네트워크 주소
* 더보기 - VSCode : terminal

### 4. dir 살펴보기 : JS, CSS, 컴포넌트

![](../../../.gitbook/assets/1%20%28135%29.png)

* create-react-app이 제공하는 기본 샘플 어플리케이션을 살펴보자 어떤 디렉토리 구조이며 어떤 파일을 수정해야하는지 알 수 있다.
* public은 서버가 실행하는 index.html이 위치한다. index.html 의 &lt;body&gt;태그에 &lt;div&gt;태그가 작성되어있는데 화면에 표시되는 모든 컴포넌트들은 저 &lt;div&gt;태그 안에 존재해야 한다.
* src는 컴포넌트들이 위치하는 경로로 대부분의 파일이 이곳에 저장된다.

![index.js](../../../.gitbook/assets/3%20%2883%29.png)

* 시작파일\(entry file\) index.js
* 11번 라인에 리액트 돔을 렌더링하는 document가 지정되어있는 것을 볼 수 있다. 'root' ID는 index.html에 작성된 div태그의 아이디임을 확인할 수 있다.
* &lt;App /&gt; html에서 제공되지 않는 태그 create-react-app이 만든 사용자 정의 태그로 import 된 App.js 컴포넌트를 가리킨다.
* App.js 를 살펴보면 index.html의 div태그안에 작성될 태그들이 작성되어있다. 실행시킨 브라우저에서 개발자도구로 코드를 살펴보면 div태그 내부의 태그들이 App.js에 작성된 내용과 같음을 볼 수 있다.

![App.js](../../../.gitbook/assets/2%20%28106%29.png)

* 리액트 돔을 구성\(렌더\)하는 &lt;App&gt; 사용자 정의 태그는 App.js를 가르키고, App.js에 작성된 render함수의 return값을 가져와 돔을 구성한다.
* 브라우저 화면을 그릴때에는  App.js에 작성해야함을 알 수 있다.

![chrome + &#xAC1C;&#xBC1C;&#xC790;&#xB3C4;&#xAD6C; + vscode](../../../.gitbook/assets/1%20%28132%29.png)

* 이렇게 App.js의 return 태그를 수정하면 create-react-app에 의해 설정된 파일이 수정 &gt; save 할 때마다 브라우저를을 자동으로 reload해준다.

![](../../../.gitbook/assets/1%20%28134%29.png)

* js 컴포넌트에 css파일을 import해 지정한다. 서버가 화면을 읽어 브라우저를 구성할때 js컴포넌트를 로드하면서 css파일도 로드해 디자인 하게된다.
* index.css는 index.js 시작문서의 css이고 App.css는 index.js가 돔 구성할때 가져오는 App.js 컴포넌트에 대한 css이다.

### 5. DEPLOY \(배포\)하기

![&#xAC1C;&#xBC1C;&#xC790;&#xB3C4;&#xAD6C; &amp;gt; Network](../../../.gitbook/assets/2%20%28107%29.png)

* 브라우저에서 강력새로고침을 하면 캐시가 모두 제거되‌고 개발자도구의 Network에서보면 1.7MB의 리소스가 다운되는 것을 확인할 수 있다.
* 리액트가 개발의 편의성을 위해 추가한 기능들에 대한 리소스로 create-react-app 개발환경은 무거운 환경임을 알 수 있다.
* 개발자 혼자 local환경에서 사용하는 것은 괜찮지만 모든 클라이언트들에게 저 1.7MB를 받게 하는것은 보안상문제를 일으킬 수 있고 문제가 될 수 있다.
* 위 문제를 피하기 위해 실행이 아닌 배포를 해야한다. 'npm run start' X 'npm run build' production mode의 어플리케이션을 만들\(빌드 할\)때에는 위 명령어를 사용한다.

![build](../../../.gitbook/assets/1%20%28136%29.png)

* 'npm run build'명령어를 통해 생성된 build 파일

## 더보기

### 앱 설치 1 : npm

* npm을 이용해 create-react-app 설치하기
* 'npm install -g create-react-app' - npm install : npm을 사용해 install 하도록 한다. - global : 컴퓨터의 어디서든 사용할 수 있도록 한다.
* npm - 프로그램 설치 앱

### 앱 설치 2 : npx 공식메뉴얼

* 리액트에서 제공하는 create-react-app 설치 공식메뉴얼
* 'npx create-react-app'
* npx  - 설치 프로그램을 임시로 설치해 단 한번만 실행 후 지운다. - 컴퓨터 공간 관리에 효율적이다. - 실행시 마다 앱을 다운로드해 항상 최신상태를 유지한다.

### error : EACCES

* 권한 없음
* 'sudo cnpm install create-react-app'
* sudo명령어로 관리자권한으로 실행하도록 한다. 관리자 계정의 암호를 물어본다.

### VSCode : terminal

* terminal 커멘드 라인\(명령어\)로 컴퓨터를 제어할 수 있는 프로그램을 내부적으로 제공한다.
* View &gt; Appearance &gt; Show Paner

### 새로고침

* 일반 새로고침 - F5
* 강력한 새로고침 - shift + F5 - 브라우저 상단의 리로드 우클릭 &gt; 캐시 비우기 및 강력 새로고침

