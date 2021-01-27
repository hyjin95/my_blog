---
description: 2021.01.27 - 111일차
---

# 111 Days -

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org, Toad DBA Suite for Oracle 11.5 , Spring, Android Studio, Visual Studio Code, Nexacro
* 사용 서버 - WAS : Tomcat

## 필기

### VirtualBox

* 가상\(소프트웨어\)로 여러대의 운영체제를 운영할 수 있다.
* [https://www.virtualbox.org/wiki/Downloads](https://www.virtualbox.org/wiki/Downloads) window hosts

### cmder

* 커맨더
* 윈도우명령어/리눅스/깃명령가능
* [https://cmder.net/](https://cmder.net/)

### ubuntu 20.04

* 직접 서버를 운영해 보는 실습
* 자바가상머신, 오라클, 톰캣서버, 깃, 파이썬
* [https://ubuntu.com/download/server\#download](https://ubuntu.com/download/server#download)

### 1. git 저장소 만들기

* .gitignore 문서에 해당 파일에 대한 등록을 진행해 이행하는데에 있어서 트러블이 발생하는 부분은배제되도록 해야 한다.
* 로컬 컴퓨터에 깃과 연동할 폴더 생성

![](../../.gitbook/assets/6%20%2823%29.png)

* 환경변수 &gt; 시스템변수 &gt; path 에 git경로 지정

### 2. 내 컴퓨터로 깃허브 저장소 클론하기 : cmder

* 로컬 컴퓨터와 깃허브 저장소 연결하기
* cmder.zip 압축 풀기 

![](../../.gitbook/assets/7%20%2815%29.png)

* 우클릭 &gt; 관리자 권한으로 실행

![](../../.gitbook/assets/0.png)

* cd.. : 경로나가기
* cd + 생성한 로컬 폴더 이름
* git clone + git clone Https url 

### 3. PyCharm Community 설정

* 파이참으로 실습 환경 설정하기

![](../../.gitbook/assets/9%20%284%29.png)

* Toolbox &gt; PyCharm Community 실행

![](../../.gitbook/assets/2%20%2890%29.png)

* File &gt; Open &gt; 로컬 깃 폴더 안에 클론되어 생성된 폴더 선택

![](../../.gitbook/assets/3%20%2867%29.png)

* File &gt; Settings &gt; Project : 프로젝트명 &gt; Python Interpreter &gt; 상단의 설정 버튼 &gt; show all

![](../../.gitbook/assets/4%20%2847%29.png)

* git에서 클론된 폴더 지정

![](../../.gitbook/assets/5%20%2833%29.png)

* Python Interpreter &gt; +\(add\) 클릭 &gt; 패키지 pip, setuptools - install Pakage

### 4. cmder 환경설정

![](../../.gitbook/assets/8%20%2810%29.png)

* cmder 상단바 우클릭 &gt; Settings
* General &gt; Fonts &gt; Main console font : 굴림체, Font Charset : Hangul

![](../../.gitbook/assets/10%20%281%29.png)

* Star up &gt; Environment set LANG=ko\_KR.UTF-8 입력 &gt; Save settings

