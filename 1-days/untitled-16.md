---
description: 2020.10.27 - 50일차
---

# 50 Days -

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5
* 사용 서버 - WAS : Tomcat

## 복습

### 웹

* main메서드가 존재하지 않는다.
* 외부에서 접근할 수 있다.
* 통신으로 소통\(듣기-요청, 말하기-응답\)할 수 있다.
* 페이지 이동이 수시로 일어난다. - 요청을 유지해야한다.  - scope가 필요하다.
* 웹 scope의 기본은 page이고, 때에따라 request, session scope를 사용한다.

### scope

* page : 해당 페이지에서만 기억
* request : 요청이 유지되는 동안 기억, 페이지를 이동하더라도 URL이 바뀌지 않는다.
* session : 브라우저에 접속되어 있는 동안 서버 측에 저장 - cookie : 소비자가 선택한 값을 클라이언트 측에 저장한다. Ex\) 장바구니
* application : 애플리케이션, 서버가 실행하는 동안 저장

{% page-ref page="untitled-1/" %}

### 로컬시스템

* main메서드가 존재한다.
* 외부에서 접근할 수 없다.
* 페이지 이동이 일어나지 않는다.

### http, https

* https - 인증서를 갖는 서버
* http - 인증받지 못한 서버

### 서블릿

* JSP보다 먼저 존재하는 자바기반 Servlet, 상속받아서 Servlet파일을 만들 수 있다.
* 자바코드안에 html을 작성할 수 있다. - view에서 입력\(듣기\)을 받을 수 있다.
* 자바이므로 인스턴스화가 가능하고, scope도 사용할 수 있다.
* 자바이므로 웹에 직접 요청 할 수 없다. - 웹에 요청하기위해 URL을 갖도록해야한다. - web.xml 배치서술자 dd파일에 등록해야한다.
* req, res객체를 WAS에게서 주입받아 내장객체로 갖는다.
* 단점 - doGet, doPost 두가지 메서드만 갖는다. - 다른 메서드를 가질 수 있기는 하지만, req와 res를 사용할 수 없다. 

### JSP

* html에 java코드를 작성할 수 있다.



