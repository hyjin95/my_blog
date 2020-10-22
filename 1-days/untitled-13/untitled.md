# Untitled

![](../../.gitbook/assets/2%20%2830%29.png)

```markup
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<!-- import 네트워트를 통해 서버의 jquery연결하기 정상적으로 찾았다면 브라우저에서 status 200이 떨어질 것이다. -->
<!-- 이 코드가 없으면 12번 라인의 alert도 실행되지 않고 개발자도구를 확인하면 $를 사용할수 없다는 등의 오류를 발견할 수 있다. -->
<script type="text/javascript" src="http://192.168.0.187:9000/js/jquery-3.5.1.min.js"></script>

<style type="text/css">
	input[type=text] {background:green;}
</style>
</head>

<body>
<!-- jquery가 지원하지 않는 API는 표준 스크립트를 사용해야 하므로 선언 수 사용을 원칙으로 하고,
 	 순서를 변경할 때에는 반드시 dom스캔 먼저 확인후에 처리하는 것을 원칙으로 해야한다.
 	 jquery는 돔 구성이 완료된 상태에서 동작한다. 10-12번 뒤에 동작한다. -->
<script type="text/javascript">
	//alert("호출이 없어도 실행됨"+$("#txt"));//JQuery는 선언이 뒤에있는 id라도 찾아준다.
	$(document).ready(function(){//DOM구성이 완료 되었을때
		$("#txt").css("color","red");
		$("#t_id").css("background","yellow");
		$("#f").attr("method","get");
		$("#f").attr("action","p169Action.jsp");
		//$("#f").submit();
	});
</script>
<p id="txt">제이쿼리로 선택자 접근하기</p>
<form id="f" action="p169Action.jsp" method="post">
	<input type="text" id="t_id" value="apple"/>
</form>
<script type="text/javascript">
	alert("무조건 실행됨");
</script>
</body>
</html>
```

![](../../.gitbook/assets/3%20%2826%29.png)

```markup
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<!-- import 네트워트를 통해 서버의 jquery연결하기 정상적으로 찾았다면 브라우저에서 status 200이 떨어질 것이다. -->
<!-- 이 코드가 없으면 12번 라인의 alert도 실행되지 않고 개발자도구를 확인하면 오류를 발견할 수 있다. -->
<script type="text/javascript" src="http://192.168.0.187:9000/js/jquery-3.5.1.min.js"></script>

<style type="text/css">
	.class0 {background:green;}
	.class1 {background:orange;}
	.class2 {background:gray;}
</style>
</head>

<body>
<!-- jquery가 지원하지 않는 API는 표준 스크립트를 사용해야 하므로 선언 수 사용을 원칙으로 하고,
 	 순서를 변경할 때에는 반드시 dom스캔 먼저 확인후에 처리하는 것을 원칙으로 해야한다.
 	 jquery는 돔 구성이 완료된 상태에서 동작한다. 10-12번 뒤에 동작한다. -->
<script type="text/javascript">
	//alert("호출이 없어도 실행됨"+$("#txt"));//JQuery는 선언이 뒤에있는 id라도 찾아준다.
	$(document).ready(function(){//DOM구성이 완료 되었을때
		//css에서 선언된 class속성을 add한다.h1이 여러개 이므로 브라우저가 배열로 인식해준다.=index를 사용해 for문 없이도 h1갯수만큼 함수가 진행된다.
		$("h1").addClass(function(index){//index가 0인 h1은 class0번 스타일이, index 1인 h1은 class1번 스타일이, ....
			return "class"+index;
		});
	});
</script>
<h1>header-0</h1>
<h1>header-1</h1>
<h1>header-2</h1>
</body>
</html>
```

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

