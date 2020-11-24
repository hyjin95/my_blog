---
description: 2020.11.24 - 70일차
---

# 70 Days -

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5
* 사용 서버 - WAS : Tomcat

## 복습 : Login

### 로그인 구현 방법

* 1 : 표준서블릿을 이용한 모델1, xxx.jsp
* 2 : 사용자 정의 서블릿을 이용하는 모델2, xxx.mem
* 공통점 : HttpServlet, 서블릿을 사용한다.

### 필요 사항

* 로그인시 url이 변하지 않고, 사용자에게 맞는 화면이 부분 제공되어야 한다.
* 비동기 통신\(ajax\)와 변수를 사용해야한다.
* 프로시서저를 호출한 결과에 대한 값을 화면에 전달한다. - 변수 : 회원이름
* 화면까지 값을 유지, 전달해야한다. - forward
* 로그인 정보를 유지하기 위해 cookie를 사용할까 session을 사용할까?

## 로그인 : session과 cookie

### 서버의 사용자 식별

* 서버가 사용자마다 session id를 부여해 식별한다.
* 클라이언트는 cookie안에 session id를 내려받아 text로 갖는다.

## 로그아웃 구현

