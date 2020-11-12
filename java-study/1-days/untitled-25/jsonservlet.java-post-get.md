# JsonServlet.java : post, get단위테스트

## JsonServlet.java + web.xml

### JsonServlet.java

```java
package book.ch17;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.apache.log4j.Logger;

public class JsonServlet extends HttpServlet {
	Logger logger = Logger.getLogger(JsonServlet.class);
	
	@Override
	public void doGet(HttpServletRequest req, HttpServletResponse res) 
		throws ServletException, IOException
	{
		logger.info("doGet 호출성공");
		res.setContentType("application/json;charset-utf_8");
		PrintWriter out = res.getWriter();
		out.print("[");
		out.print("{empno:7566, sal:3500}");
		out.print(",{empno:7567, sal:4500}");
		out.print(",{empno:7568, sal:5500}");
		out.print("]");
	}
	
	//doPost는 단위테스트가 불가능
	//url에서 직접 호출하는 것은 get방식이다, 화면이 있어야만 테스트 할 수 있다.
	@Override
	public void doPost(HttpServletRequest req, HttpServletResponse res) 
			throws ServletException, IOException
	{
		logger.info("doPost 호출성공");
		res.setContentType("application/json;charset-utf_8");
		PrintWriter out = res.getWriter();
		out.print("[");
		out.print("{\"deptno\":10, \"dname\":\"영업부\"}");
		out.print("{\"deptno\":10, \"dname\":\"영업부\"}");
		out.print("{\"deptno\":10, \"dname\":\"영업부\"}");
		out.print("]");
	}
}
```

### web.xml

```markup
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd" id="WebApp_ID" version="4.0">

<!-- DD파일(Deployment Discriptor) = 배치서술자 -->
	
	<servlet>
		<servlet-name>jsonServlet</servlet-name>
		<servlet-class>book.ch17.JsonServlet</servlet-class>
	</servlet>
	<servlet-mapping>
		<servlet-name>jsonServlet</servlet-name>
		<url-pattern>/json/jsonList</url-pattern><!-- 주소앞에는 업무명이온다. 업무마다 구분하기위해 -->
	</servlet-mapping>

</web-app>
```

## get방식 : 쿼리스트링

![&#xCFFC;&#xB9AC;&#xC2A4;&#xD2B8;&#xB9C1;&#xC73C;&#xB85C; url&#xAC80;&#xC0C9; - get&#xBC29;&#xC2DD;](../../../.gitbook/assets/1%20%2865%29.png)

## Post방식 : UI가 있어야 한다.

### b\_post.jsp

```markup
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>doPost메서드 호출하기</title>
</head>
<body>
<form id="f_test" method="post" action="/json/jsonList">
<input type="submit">	
</form>
</body>
</html>
```

* 버튼을 누르면 action을 타고 넘어가 doPost를 탄다.

