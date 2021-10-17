# Servlet 구현해보기 - Me

## Vieq : a.jsp

```java
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
    <link rel="stylesheet" type="text/css" href="https://www.jeasyui.com/easyui/themes/default/easyui.css">
    <link rel="stylesheet" type="text/css" href="https://www.jeasyui.com/easyui/themes/icon.css">
    <link rel="stylesheet" type="text/css" href="https://www.jeasyui.com/easyui/themes/color.css">
    <link rel="stylesheet" type="text/css" href="https://www.jeasyui.com/easyui/demo/demo.css">
    <script type="text/javascript" src="https://www.jeasyui.com/easyui/jquery.min.js"></script>
    <script type="text/javascript" src="https://www.jeasyui.com/easyui/jquery.easyui.min.js"></script>
<script type="text/javascript">
	var a = null;
	var form = null;
	function controll(v){
		//alert($(v).val());
		//a = $(v).val();
		form = document.getElementById("f_test");
		if(v=="select"){
			form.method="post";
			form.action="../../a/aAction.do?command=empSelect";
			form.submit();
		}
		else if(v=="insert"){
			$("#f_test").attr('method', 'post');
			$("#f_test").attr('action', '../../a/aAction.do?command=empInsert');
			$("#f_test").submit();
		}
		else if(v=="update"){//안댐
			form.method="get";
			form.action="../../a/aAction.do?command=empUpdate&timestamp="+new Date().getTime();
			form.submit();
		}
		else if(v=="delete"){
			$("#f_test").attr('method','get');
			$("#f_test").attr("action","../../a/aAction.do");
			$("#command").val('empDelete');
			$("#f_test").submit();
		}
	}
</script>
</head>
<body>
<form id ="f_test">
<!-- JQuery로는 get방식으로 넘길떄 쿼리스트링을 인정하지 않는다. hidden속성의 input을 하나 만들어서 넘기면 가져간다. -->
<input type="hidden" id="command" name="command">
<button type="button" onclick="javascript:controll('select')">조회</button>
<button type="button" onclick="javascript:controll('insert')">입력</button>
<button type="button" onclick="javascript:controll('update')">수정</button>
<button type="button" onclick="javascript:controll('delete')">삭제</button>
</form>
</body>
</html>
```

### 설명

* 31-35번은 작동하지 않는다. \
  \- JQuery에서는 get방식을 허용하지 않는 것 같다.\
  \- 이럴때에는 36-40번, 48번처럼 hidden 속성을 사용해 보이지 않는 값을 넘겨 처리할 수 있다.

## Servlet : b.jsp

```java
package practice;

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

public class a extends HttpServlet {
	
	Logger logger = Logger.getLogger(a.class);
	
	public void init() { }
	
	public void doService(HttpServletRequest req, HttpServletResponse res)
			throws ServletException, IOException{
		//테스트 해보기
		logger.info("doService 호출성공");
		
		//xxx.do?command=empInsert|empUpdate|empDelete|empSelect 이 쿼리스트링으로 
		
		//getParameter외 다른방법
		String uri = req.getRequestURI();//URL중 루트경로 다음 경로 - ?/command=empselect
		logger.info("url : "+uri);
		
		String context = req.getContextPath();//URL중 루트 경로를 가져온다. web.xml에서
		logger.info("context : "+context);

		String command = uri.substring(context.length()+1);
		logger.info(command);
		
		int end = command.lastIndexOf('.');
		logger.info(end);
		
		command = command.substring(0, end);
		logger.info(command);
		
		String works[] = null;
		works = command.split("/");
		for(String str:works) {
			logger.info(str);
		}
		
		String commands = req.getParameter("command");//empInsert|empUpdate|empDelete|empSelect 이런걸 받아와 구분하자.
		
		//사원등록할거니?
		if("empInsert".equals(commands)) {
			logger.info("empInsert호출성공");
			req.setAttribute("test", "empInsert호출ㄹㄹㄹㄹㄹㄹㄹㄹㄹㄹ");
			RequestDispatcher view = req.getRequestDispatcher("../practice/a_result.jsp");
			view.forward(req,res);			
		}		
		//사원수정해야되는데..
		else if("empUpdate".equals(commands)) {
			logger.info("empUpdate호출성공");
			req.setAttribute("test", "empUpdate호출ㄹㄹㄹㄹㄹㄹㄹㄹㄹㄹ");
			RequestDispatcher view = req.getRequestDispatcher("../practice/a_result.jsp");
			view.forward(req,res);
		}				
		//퇴사한 사원이 있는데 어떻게 하죠?
		else if("empDelete".equals(commands)) {
			logger.info("empDelete호출성공");
			req.setAttribute("test", "empDelete호출ㄹㄹㄹㄹㄹㄹㄹㄹㄹㄹ");
			RequestDispatcher view = req.getRequestDispatcher("../practice/a_result.jsp");
			view.forward(req,res);
		}				
		//사원 목록을 보고 싶어 하세요.
		else if("empSelect".equals(commands)) {
			logger.info("empSelect호출성공");
			req.setAttribute("test", "empSelectt호출ㄹㄹㄹㄹㄹㄹㄹㄹㄹㄹ");
			RequestDispatcher view = req.getRequestDispatcher("../practice/a_result.jsp");
			view.forward(req,res);
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

## view : a_result.jsp

```java
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<% String test = (String)request.getAttribute("test"); %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<%=test %>
</body>
</html>
```
