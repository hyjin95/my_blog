---
description: 2020.12.01 - 75일차
---

# 75 Days -

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5 - Spring
* 사용 서버 - WAS : Tomcat

## Spring

### web.xml

```markup
<?xml version="1.0" encoding="UTF-8"?>
<web-app version="2.5" xmlns="http://java.sun.com/xml/ns/javaee"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://java.sun.com/xml/ns/javaee https://java.sun.com/xml/ns/javaee/web-app_2_5.xsd">

	<!-- The definition of the Root Spring Container shared by all Servlets and Filters -->
	<context-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>/WEB-INF/spring/root-context.xml</param-value>
	</context-param>
```

* &lt;context-param&gt;
* 서버 기동시 한 번 읽고 유지한다.

```markup
	<!-- Creates the Spring Container shared by all Servlets and Filters -->
	<listener>
		<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
	</listener>
```

* &lt;listener&gt;태그가 있어야 Context를 읽을 수 있다.

```markup
	<!-- Processes application requests -->
	<servlet>
		<servlet-name>appServlet</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
		<init-param>
			<param-name>contextConfigLocation</param-name>
			<param-value>/WEB-INF/spring/appServlet/servlet-context.xml</param-value>
		</init-param>
		<load-on-startup>1</load-on-startup>
	</servlet>
```

* &lt;init-param&gt;
* 서블릿의 요청이 있을 때마다 새로 읽는다.

```markup
	<servlet-mapping>
		<servlet-name>appServlet</servlet-name>
		<url-pattern>*.nhn</url-pattern>
	</servlet-mapping>
</web-app>
```

* DispatcherServlet이 인터셉트 받게되는 url mapping

### SimpleUrlHandlerMapping

* 기존의 자바에서는 web.xml안에 모든 서블릿 클래스를 매핑했었다. 코드가 길어지고 가독성이 떨어져 이를 보완하기 위해 spring framwork가 제공하는 코드이다.
* 자바코드가 아닌 xml에 등록되어 존재한다.  - &lt;bean&gt;태그 안에 위치한다.
* xxx.jsp가 아닌xxx.test로 요청이 들어오면, DispatcherServlet이 인터셉트한다. SimpleUrlHandlerMapping에서 DispacherServlet이 받은  url의 클래스를 찾는다. Controller를 연결해준다.
* SimpleUrlHandlerMapping은 여기서 web.xml 에 매번 클래스를 등록하는 과정을 줄여준다. - DispatcherServlet과 Controller사이에서 url에 맞는 Controller를 찾아주는 역할이다.    

## Eclipse에서 Spring사용하기

