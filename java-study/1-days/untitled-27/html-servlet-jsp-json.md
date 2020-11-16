# html에서 비동기처리로 Servlet - jsp\(JSON\) 경유하기

## 실행화면

![](../../../.gitbook/assets/.png%20%2821%29.png)

* 실행한 문서는 a20201116\_2.jsp이지만 URL은 서블릿을 가르키고 있고, 화면은 jsonResult.jsp의 결과가 포함되어있다.
* 브라우저가 화면인 a20201116\_2.jsp를 로드할때, data도 함께 로드한다.

## 화면 : View - HTML인 JSP

### 코드 : a20201116\_2.jsp

```markup
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ page import="java.util.Map, java.util.List, java.util.HashMap" %>        
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
</head>
<body>
<script type="text/javascript">
	$(document).ready(function(){
		$("#dg_emp").datagrid({
			//url:'./empAction.jsp' //jsp에는 오라클 연동 코드를 적지 않습니다.
			url:'/erp/empCRUD.kos?work=empAction'    //서블릿에는 화면을 그리지 않습니다.
		   ,columns:[[
			   {field:'ename', title:'라벨', width:100}
			  ,{field:'gender', title:'라벨', width:100}
		   ]]
		});
	});
</script>
<table id="dg_emp" class="easyui-datagrid"></table>
</body>
</html>
```

* 시작화면인 jsp에는 직접 오라클연동페이지를 작성하지말고, servlet을 경유하는 방향을 사용하자.
* 21번에서 서블릿을 호출하고, 쿼리스트링에 업무이름을 담는다.

## Servlet : DB처리

### 코드 : ErtServlet.java

```java
package book.ch17;

public class ErpServlet extends HttpServlet{
	Logger logger = Logger.getLogger(ErpServlet.class);
	public void doService(HttpServletRequest req, HttpServletResponse res) 
	throws ServletException, IOException
	{
		logger.info("doService 호출");
		//요청 URL: /erp/empCRUD.kos?work=empAction
		String uri = req.getRequestURI();// ==> /erp/empCRUD.kos
		String context = req.getContextPath();// ==> /루트 정보를 가져옴. server.xml에서
		String command = uri.substring(context.length()+1);//==> /erp/empCRUD.kos
		int end = command.lastIndexOf('.');
		command = command.substring(0,end);
		String works[] = null;
		works = command.split("/");
		
		String work = req.getParameter("work");

		if("empAction".equals(work)) {
			logger.info("사원 등록 요청 성공");
			List<Map<String,Object>> empInfo = new ArrayList<>();
			Map<String,Object> remp = new HashMap<>();
			remp.put("ename","SMITH");
			remp.put("gender",1);
			empInfo.add(remp);
			remp = new HashMap<>();
			remp.put("ename","JOHNS");
			remp.put("gender",0);
			empInfo.add(remp);			
			req.setAttribute("empInfo", empInfo);
			RequestDispatcher view = req.getRequestDispatcher("/ASYNC/jsonResult.jsp");
			view.forward(req, res);
		}
	}
	@Override
	public void doGet(HttpServletRequest req, HttpServletResponse res) 
			throws ServletException, IOException
	{
		doService(req,res);
	}
	@Override
	public void doPost(HttpServletRequest req, HttpServletResponse res) 
			throws ServletException, IOException
	{
		doService(req,res);		
	}
}
```

* 10-16번 코드로 쿼리스트링에서 원하는 값만을 꺼낼 수 있다.
* 31번에서 request에 data를 담아 33번의 forward메서드로 32번에 지정된 jsp문서로 페이지이동한다.

## JSON : JSON인 JSP

### 코드 : jsonResult.jsp

```java
<%@ page language="java" contentType="application/json; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ page import="java.util.Map" %>    
<%@ page import="java.util.List" %>    
<%@ page import="com.google.gson.Gson" %>    
<%
	int size = 0;
	List<Map<String,Object>> empInfo = null;
  //서블릿에서는 request.setAttribute("empList",주소번지);
	empInfo = 
	(List<Map<String,Object>>)request.getAttribute("empInfo");
	if(empInfo!=null){
		size = empInfo.size();
	}
	Gson g = new Gson();
	String imsi = g.toJson(empInfo);
	out.print(imsi);
%>
```

* imsi는 JSON형태를 갖는 String data이다.

