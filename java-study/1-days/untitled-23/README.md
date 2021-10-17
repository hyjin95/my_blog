---
description: 2020.11.10 - 60일차
---

# 60 Days - Servlet과 JSP, JAVA의 역할, do-post방식, GenericServlet, HttpServlet

### 사용 프로그램

* 사용언어 : JAVA(JDK)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5
* 사용 서버 - WAS : Tomcat

## 복습

### Redirect와 forward, include

![include](<../../../.gitbook/assets/1 (62).png>)

```java
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
	//forward와 include살펴보기
	out.print("<b>before</b>");
	out.print("<hr>");

	RequestDispatcher view = request.getRequestDispatcher("b.jsp");
	view.include(request, response);
	out.print("<b>after</b>");
%>
```

* response.Redirect("b.jsp");\
  \- 해당 URL로 바로 이동한다.\
  \- 제어권이 b.jsp로 넘어간다.\
  \- 페이지 이동 후에도 다음 자바코드가 진행된다.
* RequestDispatcher view = request.getRequestDispatcher("b.jsp");\
  \- view.forward(req, res);\
    URL은 변하지 않지만 b.jsp페이지 내용을 출력한다. \
    b.jsp로 제어권이 넘어간다.\
   request에 정보를 담아 b.jsp에서도 유지시킬수 있지만 지정된 페이지에서만 유지된다.\
  \- view.include(req,res);\
    URL은 변하지 않고, b.jsp페이지 내용을 포함해서 a.jsp에서 출력한다.\
    제어권을 유지해 include다음 자바 코드를 진행한다.

### get, post방식

* 쿼리스트링을 사용하는 것은 get방식이다.
* JS(location, ajax), \<form>을 사용하면 post방식을 사용할 수 있다.

### log4j

* log4j를 활용해 콘솔에 출력된 내용들은 서버를 관리하는 사람들에게 중요한 로그로, 시스템을 운영하고 서비스를 향상시키거나 문제를 해결하는데 중요한 자료가 된다.

### 스크립틀릿 : <% 자바코드 %>

* Servlet의 서비스메서드안에 속해 사용된다.
* 메서드 안에 메서드를 정의 할 수 없으므로 스크립틀릿안에서는 메서드를 선언할 수 없다.
* 변수는 지역변수로만 선언, 사용할 수 있다.

### JSP -> JSP

* java가 아니므로 인스턴스화 할 수 없다.
* url로 호출된다.
* 모델1
* response.sendRedirect("xxx.jsp") : URL이 변하는 페이지 이동

### Servlet -> JSP : 응답

* Servlet은 Java이므로 인스턴스화가 가능하다.
* url로 호출되지만 서블릿은 자체 url이 없어 web.xml에 등록해야한다.
* Servlet에서 JSP로 페이지 이동하는 것은 Servlet이 응답을 전송해주는 것이다.

## Servlet -> Java -> Jsp : 처리 -> 응답

### 처리 -> 응답

* Serlvet이 Java의 메서드를 호출하는 것은 처리에 해당하고, Java에서 처리된 내용을 Servlet에서 foward메서드를 통해 JSP로 응답을 전송, 화면에 출력하는 것\
  \- forward는 URL은 변하지 않지만 지정된 url 페이지를 출력한다.\
  \- 요청 처리는 Java의 메서드에서 처리한다. Servlet에서 해당 메서드를 인스턴스화 해서 사용한다.\
  \- 자바에는 req, res가 없으므로
* Select(table, json, xml) 요청일때 진행되는 경로

### Servlet 역할

* Servlet의 인스턴스화를 통해 재사용성을 높이기위해 JAVA를 경유한다.
* Servlet은 res객체를 갖고 있어 response.sendRedirect를 사용할 수 있다.\
  \- 서블릿 안에서 페이지 이동을 할 수 있다.
* 왜 서블릿 안에서 페이지 이동을 해야 할까?\
  \- 서블릿으로는 화면을 구현하기 불리하므로 JSP로 화면을 구현하기위해\
  \- 인스턴스화가 가능하므로 JAVA클래스의 메서드를 호출함으로서 재사용성 높은 코드를 작성한다.

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

### JSP 직접호출, servlet경유 호출

* Servlet을 경유했는지 어떻게 알 수 있을까?\
  \- 서블릿에 Logger를 찍어 서블릿을 경유햇다면 logger.info가 출력되도록 한다.

{% content-ref url="untitled-25.md" %}
[untitled-25.md](untitled-25.md)
{% endcontent-ref %}

### 개발자가 Post방식 전송하기

```java
#ajax({ 
	 url:'xxx.do'
	,success:function(data){
		var result = JSON.stringify(data);
		var doc = JSON.parse(result);
		for(var i;i<doc.length;i++){
			//row.DEPTNO
			doc[i].DEPTNO
		}
	}
});
```

1. JS : location.href="xxx.jap"
2. JQuery : #ajax
3. easyui : \<table data-options="url:'xxx.do', method:'get' or 'post' ">\</table>
4. html : \<form id="f_login" method="get" or "post" action="xxx.do">\</form>\
   \- action = 처리 목적지, data-options의 url과 같은 역할을 한다.

## Java -> Servlet

### Java의 doGet, doPost

* main메서드를 통해 부른다.
* req, res객체를 지원하지 않는다.\
  \- locals system만을 제공해 서버가 없기 때문이다.
* 부모 : Object

### Servlet의 doGet, doPost

* 브라우저가 URL을 통해 부른다.\
  단, URL을 갖고있지않아 web.xml배치서술자 파일에 url을 등록해야한다.\
  업무이름/xxx.do
* 의미 없는 do를 붙이는 것은 do로 끝나는 모든 요청을 인터셉트하기 위함이다.
* req, res객체를 WAS로부터 주입받아 사용할 수 있다.
* 부모 : HttpServlet\
  \- HttpServlet의 최상위객체 Servlet은 Http가 없어 doGet, doPost를 사용할 수 없고 서비스만 있다.

### get, post의 단위테스트

* get과 post방식으로 전송된 요청이 제대로 실행되는지 확인하려면 단위테스트를 하는 것이 안전하다.
* get방식으로는 쿼리스트링(인터셉트)을 이용해 단위테스트 할 수 있다.
* post방식으로는 단독으로 단위테스트가 불가능하다.\
  반드시 html이나 jsp 화면이 있어야만 확인할 수 있다.\
  \- \<form>태그를 사용하거나 JS에서 post method로 지정해줘야 하기 때문이다.

{% content-ref url="java-genericservlet-httpservlet.md" %}
[java-genericservlet-httpservlet.md](java-genericservlet-httpservlet.md)
{% endcontent-ref %}

### Servlet의 LifeCycle

* init( ) -> Service( ) -> destroy( )
* init메서드에서 URL을 통한 끼어들기가 일어난다.
* Service메서드에는 doGet과 doPost메서드가 포함된다.\
  \- get을 호출하거나 post를 호출하거나 모두 service이다.
*  init에서 태어나고 service에서 수행되고, destroy에서 가비지 컬렉터에 의해 candidata상태로 변하므로 메서드 호출이나 변수사용이 불가능해진다.
* 이 라이프 사이클은 WAS서버가 관리한다. 객체주입도, 스레드에 대한 지원도 WAS가 해준다.

### WAS서버(Tomcat)

* Servlet.jar 를 포함하고 있어 HttpServlet을 지원한다.
* req, res객체를 주입해준다.

### servlet url과 web.xml

* 자바 정보를 xml에 저장한다.
* 직접 인스턴스화하지 않고 WAS가 지정된 url-pattern요청이 들어오면 인터셉트해서 해당 자바코드(Servlet)을 호출한다. = 콜백메서드화
* WAS가 서블릿엔진, jsp엔진을 갖고있기 떄문에 가능하다.
* 이렇게 서버가 읽어 로딩함으로서, 서버로부터 객체를 주입받고, 의존적이게 된다.
* web.xml에서는 서블릿을 싱글톤으로 하나만 갖고있을 수도, 여러개를 가질 수 도 있다.\
  \- 싱글톤 = 하나를 공유하는 것으로 thread가 필요한데 WAS가 이 관리를 대신 해준다.

## Servlet( I ), GenericServlet, HttpServlet

### Servlet 구현 규칙

1. main메서드를 구현하지 않는다.\
   \- JAVA의 JVM에 의해 수행되는 프로그램이 아니다.
2. Servlet, GenericServlet, HttpServlet중 하나를 상속하여 구현한다.\
   \- web서버에서 수행되는 Servlet은 HttpServlet을 상속한다. \
   \- HTTP서버인 web서버상에서 수행되는 프로그램들을 지원한다.
3. init( ), service( ), destroy( )메서드가 정해진 순서대로 호출되기 때문에 호출 시점에 수행할 기능이 있으면 해당 메서드를 오버라이딩하여 구현한다.
4. Java와는 달리 단독 수행이 불가능하다. 

### Servlet인터페이스

* Servlet 프로그램이 반드시 구현해야하는 메서드를 선언한 인터페이스
* init( ), Service( ), destroy( ), getServletinfo( ), getServletConfig( )메서드를 선언한다.
* 이 메서드들은 Servlet프로그램의 생명주기와 매핑된다.

### GenericServlet클래스

* Servlet인터페이스를 상속해 client측에서 서버단의 Application으로서 필요 기능을 구현한 추상 클래스
* Service( ) 메서드를 제외한 다른 모든 메서드들은 기능적으로 구현되어 있다.
* 어떤 프로토콜을 사용하는, 기반의 Application인지에 따라 Service메서드에 메서드 오버라이딩하여 구현한다.

### HttpServlet클래스

* GenericServlet을 상속해 service( )메서드에 HTTP프로토콜에 맞는 동작을 수행하도록 구현한 클래스
* 브라우저에게서 HTTP기반의 요청을 받아 처리하는 클래스
* service메서드에서는 요청방식 get, post에 따라 정해진 메서드를 호출하도록 구현한다.\
  \- doGet( ), doPost( )
* Web서버 기반의 servlet프로그램 개발을 이 클래스를 상속해 구현한다.
* 반드시 오버라이딩해야하지는 않고, 요청 방식에 따라 필요한 메서드를 오버라이딩한다.

후기 : html처음 할때 빡세게 이론을 정리 했던게 요새 수업을 이해하는데 도움이 되는것 같다. 코드도 많이 만져보자 해봐야 늘지!!!
