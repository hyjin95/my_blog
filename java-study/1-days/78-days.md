---
description: 2020.12.04 - 78일차
---

# 78 Days -

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5 - Spring
* 사용 서버 - WAS : Tomcat

## 클래스 조립

### 클래스 조립의 3단계

![](../../.gitbook/assets/.png%20%2843%29.png)

### Step2 : 1-2

1. ActionForward를 ModelAndView로 바꿔본다.
2. Action인터페이스에서 새로운 인터페이스를 세운다. Controller.java --Interface
3. SimpleUrlHandlerMapping대신 ControllerMapper 클래스를 설계해본다.
4. url : \*.sp2 -&gt; ActionServlet.java가 인터셉트하게 한다. ActionServlet = FrontServlet HttpServlet을 상속받는다.
5. Json형식을 출력하는 JsonController.java를 설계해본다.

### 작업 지시서

1. web.xml 문서에 ActionServlet 등록
2. mvc2.online.ActionServlet 추가
3. ModelAndView 클래스 추가\(모방해보기\) - addObject메서드와 setViewName추가
4. MemberController와 TestController추가
5. 업무 컨트롤러에는 응답페이지의 이름만 정하고 페이지 이동에 대한 지시는 ActionServlet에서 나간다.

