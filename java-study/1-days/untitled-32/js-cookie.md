# 로그인 정보 : JS로 cookie에 저장하는 과정

## 공통코드에 cookie api 추가

### 코드 : easyUI_common.jsp

```javascript
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery-cookie/1.4.1/jquery.cookie.min.js"></script>
```

* 공통코드로 include되어있는 jsp문서에 cookie를 JS에서 관리할수 있도록 해주는 스크립트 소스를 추가한다.

## 로그인시 서블릿 요청

### 코드 : index.jsp - login( )

```javascript
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<%@ include file="/common/easyUI_common.jsp" %>
<script type="text/javascript">
	function login(){
		var u_id = $("#tb_id").val();
		var u_pw = $("#tb_pw").val();
		$.ajax({
			 url:'./login.mem?work=login&mem_id='+u_id+'&mem_pw='+u_pw+'&'+new Date().getTime()
			,type:'get'
			,success:function(data){
			
			}
		});
	}/////////////////////end of login
```

* 사용자가 화면에서 로그인 버튼을 클릭하면 호출되는 login함수
* 서블릿에 get방식으로 파라미터를 넘겨 요청을 처리하도록 한다.

### 코드 : FrontController.java - doGet( )

```java
@Override
	public void doGet(HttpServletRequest req, HttpServletResponse res) 
		throws ServletException, IOException{
			logger.info("doGet 호출성공");
			String work = req.getParameter("work");
			MemberLogic memLogic = new MemberLogic();
			
			//url을 통해 업무에 대한 경우의 수를 나눈다. = if문
			if("login".equals(work)) {
				Map<String, Object> pMap = new HashMap<>();
				pMap.put("mem_id", req.getParameter("mem_id"));
				pMap.put("mem_pw", req.getParameter("mem_pw"));
				String mem_name = memLogic.login(pMap);
				logger.info("서블릿의 mem_name : "+mem_name);
				//logic->dao해서 가져온 값을 쿠키나 세션이 담아야한다.
				req.setAttribute("mem_name", mem_name);
				RequestDispatcher view = req.getRequestDispatcher("./loginAction2.jsp");
				view.forward(req, res);
			}		
```

* 파라미터를 받아 map에 담아 Logic의 메서드를 호출한다.\
  Logic은 Dao의 메서드를 호출해 myBatis로 구현된 프로시저를 호출하게된다.\
  프로시저는 파라미터로 받은 map에 응답을 처리한다.\
  결과값은 map.get("컬럼명")으로 꺼낼 수 있다.
* 처리된 값 13번을 forward로 17번에게 처리 요청한다.

### 코드 : loginAction2.jsp

```javascript
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<% 
	String mem_name = (String)request.getAttribute("mem_name");
%>
<script type="text/javascript">
	//쿠키에 이름을 저장하기 - 보안상 중요하지 않은 정보를 담는 용도로만 사용한다.
	$.cookie("cname", "<%=mem_name%>");
</script>
<%=mem_name%>
```

* 로그인이 성공했다면, 여기서 쿠키가 생성된다.

### 코드 : index.jsp - longin( )

```javascript
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<%@ include file="/common/easyUI_common.jsp" %>
<script type="text/javascript">
	function login(){
		var u_id = $("#tb_id").val();
		var u_pw = $("#tb_pw").val();
		$.ajax({
			 url:'./login.mem?work=login&mem_id='+u_id+'&mem_pw='+u_pw+'&'+new Date().getTime()
			,type:'get'
			,success:function(data){
				$("#d_login").text("");
				var imsi='';            
	            imsi+='<table border="0">';
	            imsi+='<tr><td height="30px">';
	            imsi+=data;
	            imsi+='</td></tr>';
	            imsi+='<tr><td>';
	            imsi+='<a id="btn_logout" class="easyui-linkbutton" onclick="logout()">로그아웃</a>';
	            imsi+='<a id="btn_member" class="easyui-linkbutton" onclick="memberUpdate()">정보수정</a>';            
	            imsi+='</td></tr>';
	            imsi+='</table>';
				$("#d_login").html(imsi);
			}
		});
		alert("쿠키 : "+$.cookie("cname"));
	}/////////////////////end of login
```

* 성공한 값을 html태그와 합쳐 로그인 부분을 새로 처리한다.
* 이 과정이 모두 완료되고, 브라우저가 index.jsp를 다운로드 완료하면 브라우저에서 cookie를 확인 할 수 있다. --27번
