---
description: 2020.01.04 - 96일차
---

# 96 Days -

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org, Toad DBA Suite for Oracle 11.5 , Spring, Android Studio
* 사용 서버 - WAS : Tomcat

## Spring

### 웹과 앱

* 웹 : html
* 앱 : apk

### 화면 페이지 배포 위치

* webapp &gt; board
* WEB-INF &gt; views &gt; board : url로 접근이 불가능한 배포위치
* 페이지 이동을 호출하는 구간은 Controller구간이다.
* 메서드의 파라미터 타입의 리턴 값에 따라 달라진다.
* void - req, res에 의존적 - webapp &gt; board
* ModelAndView - req, res에 의존적 - WEB-INF &gt; views &gt; board
* String  - req, res가 없어도 된다. - webapp &gt; board  **** return "redirect:xxx.jsp \| xxx.do"   return "forward:xxx.jsp" - forward는 서블릿 요청이 불가 - WEB-INF &gt; views &gt; board   return "board/boardList"와 같이 하면 ModelAndView와 동일한 위치

### 

