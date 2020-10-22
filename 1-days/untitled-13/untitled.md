# html, JSP 연결하기 - jQueryMemberShipAction

### jQueryMemberShip.html

![](../../.gitbook/assets/4%20%2820%29.png)

![](../../.gitbook/assets/5%20%2814%29.png)

```markup
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
<script type="text/javascript">
	function add(){//form전송
		$("#f_member").attr("method","get");
		$("#f_member").attr("action","memberShipAction.jsp");
		$("#f_member").submit();
	}
	function add2(){//페이지 직접 이동하기, 정보 전송은 이루어지지 않는다.
		location.href="memberShipAction.jsp";
	}
</script>
</head>
<body>
<form id="f_member">
	<table align="center" border="1" borderColor="red" width="500">
		<tr>
			<th width="150">이름 : </th>
			<td width="350">
			<input type="text" size="12">
			<div id="d_name"></div>
			</td>
		</tr>
		<tr>
			<th width="150">아이디 : </th>
			<td width="350">
			<input type="text" size="12">
			<div id="mem_id"></div>
			</td>
		</tr>
		<tr>
			<th width="150">패스워드 : </th>
			<td width="350">
			<input type="text" size="12">
			<div id="mem_pw"></div>
			</td>
		</tr>
		<tr>
			<th width="150">패스워드확인 : </th>
			<td width="350">
			<input type="text" size="12">
			<div id="mem_pw_check"></div>
			</td>
		</tr>
		<tr>
			<th width="100%" colspan="2" align="center">
			<button onClick="add()">가입</button>
			<button onClick="cancel()">취소</button>
			</th>
		</tr>
	</table>
</form>
</body>
</html>
```

### memberShipAction.jsp

```markup
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
	//스크립틀릿 : 자바코딩을 할 수 있는 영역
	String mem_id = request.getParameter("mem_id");
	//if, for 제어문, 사칙연산(알고리즘, 업무처리(비즈니스로직))을 작성한다. 
	response.sendRedirect("result.jsp");//화면출력은 다른 jsp페이지에서 해야한다.
%>
<!-- html영역 -->
```

### result.jsp

```markup
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>응답페이지 입니다.</title>
</head>
<body>

</body>
<
```

