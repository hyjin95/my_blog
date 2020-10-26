# TestController.java - Get, Post방식

## TestController.java

```java
package web.mvc;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.apache.log4j.Logger;

//Servlet파일
public class TestController extends HttpServlet {

	Logger logger = Logger.getLogger(TestController.class);
	
	private static final long serialVersionUID = 1L;
	/**************************************************************************
	 * @param : req - 톰캣 서버가 제공해주는 객체 - Servlet-api.jar
	 * @param : res - 톰캣 서버가 제공해주는 객체 - Servlet-api.jar
	 * 메서드 이름 뒤에 throws로 예외를 던지면 예외발생시 해당 메서드를 호출한 곳에서 처리한다.
	 * 이 서블릿은 톰캣서버가 싱글톤패턴으로 직접 관리한다. 내가 예외처리 할 수 없다. 불가능 - 스레드도, 통신도 톰켓이 관리
	 * doGet은 일종의 콜백메서드이다.
	 * 질문 : 나는 자바코드인데 브라우저에서 실행시키고 싶을때는 어떡해야할까?
	 * 힌트 : 나는 메인메서드가 없다.
	 * 	        나는 url도 없다.
	 * 		 나는 doGet메서드만 있다.
	 * 방법 : xml, 배치서술자 dd파일에 servlet class를 추가한다.	 * 
 	 *************************************************************************/
	//기본 전송방식
	public void doGet(HttpServletRequest req, HttpServletResponse res)
			throws ServletException, IOException
	{
		logger.info("doGet 호출성공");
		//응답객체를 통해 마임타입을 지정할 수 있고, 한글 인코딩도 추가할 수 있다.
		res.setContentType("text/html;charset=utf-8");
		PrintWriter out = res.getWriter();
		//브라우저에 쓰기 
		out.print("<b>서블릿으로 그린 웹페이지 입니다.</b>");//java에 html코드를 작성할 수 있다. servlet
	}
	//post방식으로 전송하려면 반드시 <form>이나 JS를 사용해야한다.
	public void doPost(HttpServletRequest req, HttpServletResponse res)
			throws ServletException, IOException
	{
		logger.info("doPost 호출성공");
		//응답객체를 통해 마임타입을 지정할 수 있고, 한글 인코딩도 추가할 수 있다.
		res.setContentType("text/html;charset=utf-8");
		PrintWriter out = res.getWriter();
		out.print("<b>Post전송으로 요청된 웹페이지 입니다.</b>");
	}
}
```

* java로 Servlet을 상속받아 Servlet파일을 생성해 post, get방식 전송 메서드를 만들어 요청에 따라 해당 메서드를 진행한다. 
* 기본 전송방법은 get방식

## web.xml

```markup
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd" id="WebApp_ID" version="4.0">
<!-- 
sever.xml은 톰캣서버가 기동할떄 디폴트, 기본으로 읽게되는데
xml의 규칙 내에서 포트번호를 결정하고 프로젝트를 배치한다. 
-->
<!-- log4j 환경파일 등록하기 -->
	<context-param>
		<param-name>log4jConfigLocation</param-name><!-- 객체주입 -->
		<param-value>/WEB-INF/classes/log4j.properties</param-value><!-- 톰캣서버가 읽을 수 있게 한다. -->
	</context-param>
<!-- DD파일(Deployment Discriptor) = 배치서술자 -->
	<servlet>
		<servlet-name>testMgr</servlet-name>
		<servlet-class>web.mvc.TestController</servlet-class>
	</servlet>
	<servlet-mapping>
		<servlet-name>testMgr</servlet-name>
		<url-pattern>/test/test.do</url-pattern><!-- url -->
		<!-- 웹서비스를 할때에 자바를 인스턴스화할 수 없으므로 url로 접근한다. 이를 위해 xml dd파일에 url을 생성해야한다. -->
	</servlet-mapping>
</web-app>
```

* Servlet을 사용해서 자바의 웹 서버를 사용하기 위해서는 web.xml, DD파일\(배치서술자\)에 Servlet class를 작성해 url을 만들어 주어야 한다.

## memberShip.html

```markup
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>memberShip.html</title>
</head>
<body>
<form method="post" action="/test/test.do">
<!-- 전송해주는 버튼이 필요 submit-->
<input type="submit" value="전송">
</form>
</body>
</html>
```

* Post방식을 사용하려면 반드시 &lt;form&gt;태그로 감싸 전송하거나, JS를 사용해야만한다.

