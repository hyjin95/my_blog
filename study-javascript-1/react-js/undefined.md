---
description: 2021.03.29 - 월요일
---

# 리액트 개발환경

## 개발환경

### 1. npm 설치 및 확인

* Node js에서 LTS 안정화된 버전을 설치한다.
* CMD / 터미널에서 'node -v' 명령어로 버전을 확인해 설치여부를 알 수 있다. 마찬가지로 'npm -v'명령어로 npm 설치여부를 확인한다.
* 더보기란 확인 : 앱설치 1, 2 'create-react-app -V' 명령어로 설치확인 \(V는 대문자\) 

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

