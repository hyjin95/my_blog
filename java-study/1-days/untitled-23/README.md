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

### Servlet으로 화면을 그리지 않는 이유

```java
public void doService(HttpServletRequest req, HttpServletResponse res)
			throws ServletException, IOException{
		//테스트 해보기
		logger.info("doService 호출성공");
		//첫번쨰 조건, printWriter가 있어야 out을 사용할 수 있다.
		//ObjectOutputStream oos = socket.getOutputStream();과 비슷, res객체로 적기 를 만든다
		PrintWriter out = res.getWriter();
		out.print("<html>");
		out.print("<head><title>사원관리</title></head>");
		out.print("<body>");
		out.print("으악 이게모람");
		out.print("<h2>내용이 오는 자리</h2>");
		out.print("<div id='d_msg'>내용이 오는 자리<div>");
		out.print("</body>");
		out.print("</html>");
	}
```

* html코드를 작성, 출력할 수는 있지만 아주 번거롭다.

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

{% page-ref page="java-genericservlet-httpservlet.md" %}

### Servlet의 LifeCycle

* init\( \) -&gt; Service\( \) -&gt; destroy\( \)
* init메서드에서 URL을 통한 끼어들기가 일어난다.
* Service메서드에는 doGet과 doPost메서드가 포함된다. - get을 호출하거나 post를 호출하거나 모두 service이다.
*  init에서 태어나고 service에서 수행되고, destroy에서 가비지 컬렉터에 의해 candidata상태로 변하므로 메서드 호출이나 변수사용이 불가능해진다.
* 이 라이프 사이클은 WAS서버가 관리한다. 객체주입도, 스레드에 대한 지원도 WAS가 해준다.

### WAS서버\(Tomcat\)

* Servlet.jar 를 포함하고 있어 HttpServlet을 지원한다.
* req, res객체를 주입해준다.

## Servlet\( I \), GenericServlet, HttpServlet

### Servlet 구현 규칙

1. main메서드를 구현하지 않는다. - JAVA의 JVM에 의해 수행되는 프로그램이 아니다.
2. Servlet, GenericServlet, HttpServlet중 하나를 상속하여 구현한다. - web서버에서 수행되는 Servlet은 HttpServlet을 상속한다.  - HTTP서버인 web서버상에서 수행되는 프로그램들을 지원한다.
3. init\( \), service\( \), destroy\( \)메서드가 정해진 순서대로 호출되기 때문에 호출 시점에 수행할 기능이 있으면 해당 메서드를 오버라이딩하여 구현한다.
4. Java와는 달리 단독 수행이 불가능하다. 

### Servlet인터페이스

* Servlet 프로그램이 반드시 구현해야하는 메서드를 선언한 인터페이스
* init\( \), Service\( \), destroy\( \), getServletinfo\( \), getServletConfig\( \)메서드를 선언한다.
* 이 메서드들은 Servlet프로그램의 생명주기와 매핑된다.

### GenericServlet클래

* Servlet인터페이스를 상속해 client측에서 서버단의 Application으로서 필요 기능을 구현한 추상 클래스
* Service\( \) 메서드를 제외한 다른 모든 메서드들은 기능적으로 구현되어 있다.
* 어떤 프로토콜을 사용하는, 기반의 Application인지에 따라 Service메서드에 메서드 오버라이딩하여 구현한다.

### HttpServlet클래스

* GenericServlet을 상속해 service\( \)메서드에 HTTP프로토콜에 맞는 동작을 수행하도록 구현한 클래스
* 브라우저에게서 HTTP기반의 요청을 받아 처리하는 클래스
* service메서드에서는 요청방식 get, post에 따라 정해진 메서드를 호출하도록 구현한다. - doGet\( \), doPost\( \)
* Web서버 기반의 servlet프로그램 개발을 이 클래스를 상속해 구현한다.
* 반드시 오버라이딩해야하지는 않고, 요청 방식에 따라 필요한 메서드를 오버라이딩한다.

