---
description: 2020.11.10 - 60일차
---

# 60 Days -

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5
* 사용 서버 - WAS : Tomcat

## 복습

### 스크립틀릿 : &lt;% 자바코드 %&gt;

* Servlet의 서비스메서드안에 속해 사용된다.
* 메서드 안에 메서드를 정의 할 수 없으므로 스크립틀릿안에서는 메서드를 선언할 수 없다.
* 변수는 지역변수로만 선언, 사용할 수 있다.

### JSP -&gt; JSP

* java가 아니므로 인스턴스화 할 수 없다.
* url로 호출된다.
* 모델1
* response.sendRedirect\("xxx.jsp"\) : URL이 변하는 페이지 이동

### Servlet -&gt; JSP : 응답

* Servlet은 Java이므로 인스턴스화가 가능하다.
* url로 호출되지만 서블릿은 자체 url이 없어 web.xml에 등록해야한다.
* Servlet에서 JSP로 페이지 이동하는 것은 Servlet이 응답을 전송해주는 것이다.

## Servlet -&gt; Java -&gt; Jsp : 처리 -&gt; 응답

### 처리 -&gt; 응답

* Serlvet이 Java의 메서드를 호출하는 것은 처리에 해당하고, Java에서 처리된 내용을 Servlet에서 foward메서드를 통해 JSP로 응답을 전송, 화면에 출력하는 것 - forward는 URL은 변하지 않지만 JSP페이지를 출력한다.
* Select\(table, json, xml\) 요청일때 진행되는 경로

### Servlet 역할

* Servlet의 인스턴스화를 통해 재사용성을 높이기위해 JAVA를 경유한다.
* Servlet은 res객체를 갖고 있어 response.sendRedirect를 사용할 수 있다. - 서블릿 안에서 페이지 이동을 할 수 있다.
* 왜 서블릿 안에서 페이지 이동을 해야 할까? - 서블릿으로는 화면을 구현하기 불리하므로 JSP로 화면을 구현하기위해 - 인스턴스화가 가능하므로 JAVA클래스의 메서드를 호출함으로서 재사용성 높은 코드를 작성한다.

## Java -&gt; Servlet

### Java의 doGet, doPost

* main메서드를 통해 부른다.
* req, res객체를 지원하지 않는다. - locals system만을 제공해 서버가 없기 때문이다.
* 부모 : Object

### Servlet의 doGet, doPost

* 브라우저가 URL을 통해 부른다. 단, URL을 갖고있지않아 web.xml배치서술자 파일에 url을 등록해야한다. 업무이름/xxx.do
* 의미 없는 do를 붙이는 것은 do로 끝나는 모든 요청을 인터셉트하기 위함이다.
* req, res객체를 WAS로부터 주입받아 사용할 수 있다.
* 부모 : HttpServlet - HttpServlet의 최상위객체 Servlet은 Http가 없어 doGet, doPost를 사용할 수 없고 서비스만 있다.

### Servlet의 LifeCycle

* init\( \) -&gt; Service\( \) -&gt; destroy\( \)
* init메서드에서 URL을 통한 끼어들기가 일어난다.
* Service메서드에는 doGet과 doPost메서드가 포함된다.

### WAS서버\(Tomcat\)

* Servlet.jar 를 포함하고 있어 HttpServlet을 지원한다.
* req, res객체를 주입해준다.

## Java, Servlet, GenericServlet, HttpServlet

