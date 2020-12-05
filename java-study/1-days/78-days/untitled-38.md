# onLineTest

## xml

### 코드 : web.xml

```markup
<servlet-name>MVC2</servlet-name>
		<servlet-class>mvc2.online.ActionServlet</servlet-class>
	</servlet>
<servlet-mapping>
		<servlet-name>MVC2</servlet-name>
		<url-pattern>*.sp2</url-pattern><!-- 주소앞에는 업무명이온다. 업무마다 구분하기위해 -->
</servlet-mapping>
```

## Interface

### 코드 : Controller.java

```java
package mvc2.online;

import java.io.IOException;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public interface Controller {
	
	public ModelAndView execute(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException;
	
}
```

## ControllerMapper

### 코드 : ControllerMapper.java

```java
package mvc2.online;

import org.apache.log4j.Logger;

public class ControllerMapper {
	Logger logger = Logger.getLogger(ControllerMapper.class);
	
	public static Controller getController(String command) {
		Controller controller = null;
		String commands[] = command.split("/");
		if(commands.length == 2) {
			String work = commands[0];//폴더이름, 업무이름
			String requestName = commands[1];//매서드이름, 업무내용명 -> 응답페이지 call
			
			if("login".equals(work)) {
				controller = new MemberController(requestName);
			}
			if("onLineTest".equals(work)) {
				controller = new TestController(requestName);
			}
		}
		return controller;
	}
}
```

## Servlet

### 코드 : ActionServlet.java

```java
package mvc2.online;

import java.io.IOException;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletConfig;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.apache.log4j.Logger;

//init():시스템 - service():개발자 - destroy():시스템
//시스템 = ServletContainer = Tomcat
public class ActionServlet extends HttpServlet {
	Logger logger = Logger.getLogger(ActionServlet.class);
	
	@Override
	public void init(ServletConfig config) throws ServletException {
		//해당 서블릿의 초기화 담당 
		//예를 들어 오라클 서버의 물리적인 정보들 DataSource, Connection 물리기 위한 사전 정보들을 초기화하는 작업
		logger.info("init 호출성공");
	}
	
	public void doService(HttpServletRequest req, HttpServletResponse res) 
			throws ServletException, IOException{
		logger.info("doService 호출성공");
		String uri = req.getRequestURI();// /login/memberList.sp2
		String context = req.getContextPath();
		String command = uri.substring(context.length()+1);
		int end = command.lastIndexOf(".");
		command = command.substring(0, end);
		logger.info("command : "+command);
		Controller controller = null;
		try {
			controller = ControllerMapper.getController(command);
		} catch (Exception e) {
			// TODO: handle exception
		}
		if(controller instanceof MemberController) {//타입이 MemberController니?
			logger.info("회원 컨트롤 계층 호출성공");
			ModelAndView mav = controller.execute(req, res);
			RequestDispatcher view = req.getRequestDispatcher(mav.viewName);
			view.forward(req, res);
		}
		if(controller instanceof TestController) {
			logger.info("시험 컨트롤 계층 호출성공");
			ModelAndView mav = controller.execute(req, res);
			logger.info("mav : "+mav+", viewName : "+mav.viewName);
			RequestDispatcher view = req.getRequestDispatcher(mav.viewName);
			view.forward(req, res);
		}
	}
	
	@Override
	public void doGet(HttpServletRequest req, HttpServletResponse res) 
		throws ServletException, IOException{
			logger.info("doGet 호출성공");
			doService(req,res);
		}
	
	@Override
	public void doPost(HttpServletRequest req, HttpServletResponse res) 
		throws ServletException, IOException{
			logger.info("doPost 호출성공");
			doService(req,res);
	}
	
	public void destroy() {
		
	}
}
```

## getter, setter

### 코드 : ModelAndView.java

```java
package mvc2.online;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.apache.log4j.Logger;

public class ModelAndView {
	Logger logger = Logger.getLogger(ModelAndView.class);
	HttpServletRequest request = null;
	HttpServletResponse response = null;
	
	String viewName = null;
	List<Map<String,Object>> reqList = new ArrayList<>();
	
	public ModelAndView() {	}
	
	public ModelAndView(HttpServletRequest request) {	
		this.request = request;
	}
	
	public ModelAndView(HttpServletRequest request, HttpServletResponse response) {	
		this.request = request;
		this.response = response;
	}
	
	/***************************************************************************************
	 * ModelAndView mav = new ModelAndView();
	 * mav.setViewName(login/login);
	 * @param viewName
	 ***************************************************************************************/
	public void setViewName(String viewName) {//응답페이지로 나갈 페이지 이름
		//String commands[] = viewName.split("/");
		logger.info("viewName : "+viewName);
		this.viewName = viewName;
	}
	
	public void addObject(String name, Object obj) {//scope가 reqeust일때 값을 유지하는 메서드
		//여러개의 값을 추가하는 코드
		Map<String, Object> rMap = new HashMap<>();
		rMap.put(name, obj);
		reqList.add(rMap);
		//아래 코드는 오직 한개만 처리 가능
		//request.setAttribute(name, obj);
		request.setAttribute("reqList", reqList);
	}
}
```

## Controller

### 코드 : MemberController.java

```java
package mvc2.online;

import java.io.IOException;
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.apache.log4j.Logger;

public class MemberController implements Controller {
	Logger logger = Logger.getLogger(MemberController.class);
	String requestName = null;
	
	public MemberController(String requestName) {
		//login.memberList.sp2 -> memberList만 추출
		//확장자 sp2는 뗴어낸다.
		this.requestName = requestName;
	}

	@Override
	public ModelAndView execute(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		logger.info("excute 호출성공 "+requestName);
		List<Map<String,Object>> memList = new ArrayList<>();
		Map<String,Object> rmap = new HashMap<>();
		rmap.put("mem_id", "tomato");
		rmap.put("mem_id", "apple");
		memList.add(rmap);
		ModelAndView mav = new ModelAndView(request,response);
		mav.addObject("memList", memList);
		mav.setViewName(requestName+".jsp");
		return mav;
	}
}
```

### 코드 : TestController.java

```java
package mvc2.online;

import java.io.IOException;
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.apache.log4j.Logger;

public class TestController implements Controller {
	Logger logger = Logger.getLogger(TestController.class);
	String requestName = null;
	
	public TestController(String requestName) {
		this.requestName = requestName;
	}

	@Override
	public ModelAndView execute(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		logger.info("excute 호출성공 "+requestName);
		//List<Map<String,Object>> testList = new ArrayList<>();
		ModelAndView mav = new ModelAndView(request,response);
		//mav.addObject("testList", testList);
		mav.setViewName(requestName+".jsp");
		return null;
	}
}
```

## 응답페이지

### 코드 : memberList.jsp

```markup
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ page import = "java.util.List, java.util.Map" %>
    <!-- rul : /login/memberList.sp2 -> login/memberList.jsp
    	 forward 페이지 이동이므로 url은 변하지 않는다. -->
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>회원목록 페이지</title>
</head>
<body>
회원 목록페이지 입니다.
<%
	List<Map<String,Object>> memList = (List<Map<String,Object>>)request.getAttribute("memList");
	out.print(memList);
%>
</body>
</html>
```

### 코드 : testList.jsp

```markup
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>시험문제 목록 페이지</title>
</head>
<body>
testList.jsp페이지 입니다.
</body>
</html>
```

