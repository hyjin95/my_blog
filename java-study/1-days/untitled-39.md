---
description: 2020.12.08 - 80일차
---

# 80 Days -

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5 - Spring
* 사용 서버 - WAS : Tomcat

## 필기

### 웹 서비스 제공

* 주체 : 브라우저 - url주소 -&gt; http\(java처리\) -&gt; url로 업무분기\(if\)
* url : /프로젝트명/work name/업무이름.url-pattern
* java로 처리된다 = 안드로이드와의 연계가 가능하다.

## POJO

### POJO 1-1

* url : \*.test
* url인터셉트 : DispatcherServlet -&gt; FrontMVC1.java

### POJO 1-2

* url : \*.sp2
* url인터셉트 : DispatcherServlet -&gt; ActionServlet.java

### POJO 1-3

* url : \*.sp3
* url인터셉트 : DispatcherServlet -&gt; ActionSupport.java
* ModelAndView3를 등록해 다중등록이 가능하도록 해보자.

### 공통점

* req, res에 의존적이다. - 없으면 사용할 수 없게 된다.\(sendRedirect, getAttribute, ...\)
* 페이지 이동에 대한 처리설계가 필요하다. - sendRedirect, forward

### 차이점

* url패턴이 다르다.
* return type 변경 - 표준 : void - settet, getter : ActionForward - ModelAndView\(1-2\)   jsp위치 주의 : webContent or WEB-INF - String\(1-3\)   redirect:xxx.jsp or forward:xxx.jsp   ' : '를 기준으로 문자열을 분리해 처리한다.   jsp위치 주의 : webContent or WEB-INF

## Final Project

### 개발

* Back-End -&gt; Front\_End - xml -&gt; Java -&gt; 화면
* DML SQL문을 반드시 먼저 테스트 해야한다.
* PL : 복잡한 쿼리문\(조인, 프로시저, 인라인뷰, 서브쿼리, 집계, 통계\)작성을 미리 한다.
* PM : crew관리
* 형상관리 담당자 한명 배정 : git관리, 운영, 테스트

