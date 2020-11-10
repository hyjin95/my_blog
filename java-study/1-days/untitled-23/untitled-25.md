# Untitled

## web.xml

```markup
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd" id="WebApp_ID" version="4.0">

<!-- DD파일(Deployment Discriptor) = 배치서술자 -->	
	<servlet>
		<servlet-name>A3Servlet</servlet-name>
		<servlet-class>com.basic.A3</servlet-class>
	</servlet>
	<servlet-mapping>
		<servlet-name>A3Servlet</servlet-name>
		<url-pattern>/EasyUI/a3.do</url-pattern><!-- 주소앞에는 업무명이온다. 업무마다 구분하기위해 -->
	</servlet-mapping>

</web-app>
```

* 11번 - 서블릿 : /EasyUI/a3.do - JSP : dev\_html - webcontent - EasyUI - ...

### JSP 가설세워보기

1. dev\_html - webcontent - EasyUI - xxx.jsp
2. dev\_html - webcontent - EasyUI - datagrid - emp - xxx.jsp
3. dev\_html - webcontent - EasyUI - index.jsp 

위 세가지 가설을 모두 생각해보고 콘솔을 통해 확인해보자

## A3.java

```java
package com.basic;

import java.io.IOException;
import javax.servlet.GenericServlet;
import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import org.apache.log4j.Logger;

public class A3 extends HttpServlet {	
	Logger logger = Logger.getLogger(A3.class);
	
	public void init() { }
	
	public void doService(HttpServletRequest req, HttpServletResponse res)
			throws ServletException, IOException{
		//테스트 해보기
		logger.info("doService 호출성공");
		//res.sendRedirect("a3_result.jsp");
		RequestDispatcher view = req.getRequestDispatcher("a3_result.jsp");
		view.forward(req,res);
		//이 아래 코드는 진행이 될까?아니요 불가능 a3_result.jsp에서 응답을 하고 끝난다.
		
		//xxx.do?command=empInsert|empUpdate|empDelete|empSelect 이 쿼리스트링으로 
		String command = req.getParameter("command");//empInsert|empUpdate|empDelete|empSelect 이런걸 받아와 구분하자.
		logger.info("command : "+command);
		//사원등록할거니?
		if("empInsert".equals(command)) {
			
		}		
		//사원수정해야되는데..
		else if("empUpdate".equals(command)) {
			
		}				
		//퇴사한 사원이 있는데 어떻게 하죠?
		else if("empDelete".equals(command)) {
			
		}				
		//사원 목록을 보고 싶어 하세요.
		else if("empSelect".equals(command)) {
			
		}		
		
		//위 목록을 if, else if문으로 처리하면 코드가 더러워진다. -> controller mapper를 사용하자
		//목록이 많아질수록 if문이 엄청 길어질 수 도 있으므로
	}
	@Override
	public void doGet(HttpServletRequest req, HttpServletResponse res) 
		throws ServletException, IOException{
			logger.info("doGet 호출성공");
			doService(req, res);
		}
	
	@Override
	public void doPost(HttpServletRequest req, HttpServletResponse res) 
		throws ServletException, IOException{
			logger.info("doPost 호출성공");
			doService(req, res);
	}
	
	public void destroy() {	}	
}
```

