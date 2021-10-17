---
description: 2021.03.26 금요일
---

# Node JS & React JS : Bolier-Plate

## 강의

| 분류       | 사용                 |
| -------- | ------------------ |
| 언어       | Node JS, React JS  |
| FramWork | EXPRESS JS         |
| DB       | MongoDB            |
| tool     | Visual Studio Code |

노드 리액트로 보일러 팔레트 만들기[https://edu.goorm.io/learn/lecture/17855/%EB%94%B0%EB%9D%BC%ED%95%98%EB%A9%B0-%EB%B0%B0%EC%9A%B0%EB%8A%94-%EB%85%B8%EB%93%9C-%EB%A6%AC%EC%95%A1%ED%8A%B8-boiler-plate-%EB%A7%8C%EB%93%A4%EA%B8%B0](https://edu.goorm.io/learn/lecture/17855/%EB%94%B0%EB%9D%BC%ED%95%98%EB%A9%B0-%EB%B0%B0%EC%9A%B0%EB%8A%94-%EB%85%B8%EB%93%9C-%EB%A6%AC%EC%95%A1%ED%8A%B8-boiler-plate-%EB%A7%8C%EB%93%A4%EA%B8%B0)

### Bolier-Plate

* 로그인 같은 기능들은 자주 사용되는 기능 중 하나이다.\
  어떤 프로젝트에서든 공통으로 재사용할 수 있게 만들어 두는 것

### Node JS

* node가 나오기전에는 JS는 클라이언트 사이드(브라우저)에서만 사용할 수 있는 언어였고 node가 나옴으로서 서버사이드에서 JS를 사용할 수 있게 되었다.
* JS 서버사이드 엔진
* Vue.js의 경우 node를 설치한 후 패키지 관리자(npm)의 Vue CLI를 사용하면 Vue의 기본적인 개발 환경을 빠르게 구성하고 테스트하기 용이해 실제 node와 Vue를 통합해 사용하는 경우가 많다.

### Express JS

* node JS라는 언어를 좀 더 쉽게 사용할 수 있게 해주는 프레임워크\
  node JS로 프로그램을 만들수 있게 해준다.

### npm

* node package manager\
  Node.js설치시 자동 설치되는 Node.js의 기본 패키지 관리자
* JS코드를 구성하고 공유하는 도구\
  JS로 된 모든 것을 패키지로 관리한다. JS F/W를 사용하는 프로젝트에서 필수 도구
* Vue의 경우 npm을 통해 기본 프로젝트를 구성해주는 Vue CLI패키지를 배포하고있다.

## 설치 및 세팅

### Node JS & Express JS 

![](<../.gitbook/assets/1 (131).png>)

* Node JS\
  [https://nodejs.org/ko/](https://nodejs.org/ko/)\
  최신버전이 아닌 검증된 LTS버전을 다운받는다.
* CMD에서 'node -v' 명령어로 설치, 버전을 확인한다.

![](<../.gitbook/assets/2 (103).png>)

* Bolier-Plate를 생성할 디렉토리를 생성한다.

![](<../.gitbook/assets/3 (80).png>)

* 해당 디렉토리에서 'npm init'명령어로 npm 패키지를 생성한다.
* 전부 기본값으로  enter하고 author만 작성자를 명시해준다.
* enter

![](<../.gitbook/assets/1 (130).png>)

* text Editor인 Visual Sudio Code에서 만들어진 폴더를 열어보자
* npm으로 만든 json형식의 패키지 파일이 생성된 것을 확인 할 수 있다.

![](<../.gitbook/assets/2 (105).png>)

* CMD또는 위처럼 visual studio code의 terminal 에서 'npm install express --save' 명령어를 통해 express JS를 다운로드 한다.

![](<../.gitbook/assets/3 (79).png>)

* \--save 명령어를 사용하면 package.json에 다운로드된(사용하는) 라이브러리가 명시되어 프로젝트를 공유하는 팀원들이 확인할 수 있다.

![](<../.gitbook/assets/4 (56).png>)

* 생성된 node_modules 폴더는 dependencye들을 관리한다.

![index.js](<../.gitbook/assets/1 (128).png>)

* [https://expressjs.com/en/starter/hello-world.html](https://expressjs.com/en/starter/hello-world.html)
* 1번 : express 모듈을 가져온다.
* 2번 : express( ) 함수로 새 express를 생성한다.
* 3번 : 서버 port번호 지정
* 5-7번 : 생성된 express로 root 디렉토리('/')에 'Hello World' 응답을 출력한다.
* 9-11번 : 생성된 express에 port번호를 지정해 해당 port번호에서 앱을 실행하게 한다.

![](<../.gitbook/assets/2 (104).png>)

* 패키지 파일의 scripts에 "start"로 index.js를 등록한다.

![](<../.gitbook/assets/3 (81).png>)

* 파일 저장 후 터미널에 'npm run start'명령어를 입력하면 npm은 script에 등록된 "start"를 run(실행)하게된다.
* start로 지정된 node index.js를 run 하면 index.js가 요청을 읽어(listen) 지정된 포트번호로 서버를 시작한다.

![](<../.gitbook/assets/1 (129).png>)

* 브라우저에서 확인하기
* index.js문서를 수정시 터미널에서 서버를 다운시켰다가 재 가동후 브라우저를 새로고침해야 반영된다.
