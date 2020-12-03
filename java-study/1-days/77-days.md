---
description: 2020.12.03 - 77일차
---

# 77 Days -

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5 - Spring
* 사용 서버 - WAS : Tomcat

## 온라인 시험 사전 정의서

### 오늘의 학습목표

1. 화면\(View\)에 대한 Flow Chart를 작성한다. - 어떤 페이지가 필요한지 생각해본다. - 화면 정의서와 연결된다.
2. 업무 정의서\(규칙\)를 작성한다. - Model + Dao
3. 쿠키\(서블릿\) API를 활용할 수 있다. - 쿠키 : 정보의 유지, 클라이언트, text, response - 화면과 쿠키의 연결 - JS가 아닌 JAVA로 처리한다. - 서블릿\(jsp, servlet\)안에서 쿠키 객체를 생성한다.
4. 개발방법론\(MVC\) - 모델1, 모델2

### 온라인 시험 업무 정의서

1. 수험 번호와 이름 입력 --onLineTest.html\(View\)
2. 인증처리 - select + where + and - 수험번호 일치 and 이름 일치 - 사용자 정보를 cookie에 담아 유지한다.
3. 문제 페이지 - 첫번째 페이지는 html로 하되 2-5번째 문제 페이지는 jsp로 해야한다. - response를 활용해 사용자가 선택한 답을 내보내야 하기 때문이다. - 5번째 문제까지도 고른 답들을 유지해야하므로 url이 변하는 페이지 이동은 안된다.
4. 제출 - 제출 클릭시 페이지 이동이 발생하지 않으면, 서버와 클라이언트의 쿠키가 동기화 되지않는다.

### Page Flow Chart

![](../../.gitbook/assets/.png%20%2843%29.png)

