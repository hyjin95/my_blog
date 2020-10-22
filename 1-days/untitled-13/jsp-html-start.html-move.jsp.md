# JSP, HTML 연결 - start.html, move.jsp

## 1단계 : start.html

```markup
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<input type="text" id="mem_id">
<a href="move.jsp">이동</a>
</body>
</html>
```

* 기본형태

![](../../.gitbook/assets/2%20%2830%29.png)

* 이동 링트에 커서를 올리면 하단에 이동할 주소가 나타난다.

![](../../.gitbook/assets/3%20%2823%29.png)

* 아직 move.jsp파일이 없으므로 404가 발생하고, URL이 변경된 것을 볼 수 있다.
* Session연결이 끊겼다가 새 연결이 된 것이다.
* 이렇게 되면 request가 담은 사용자의 입력값을 유지할 수 없다.
* 유지하려면 쿼리스트링을 이용하는 get방식을 이용한다.

## 2단계 : get방식으로 요청 유지하기 

![](../../.gitbook/assets/4%20%2818%29.png)

```markup
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script type="text/javascript">
	function move(){
		location.href="move.jsp?mem_id= "+document.getElementById("mem_id").value;
	}
</script>
</head>
<body>
<input type="text" id="mem_id">
<a href="move.jsp?mem_id= tomato">이동</a>
</body>
</html>
```

![](../../.gitbook/assets/5%20%2813%29.png)

```markup
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
	String mem_id = request.getParameter("mem_id");
	out.print("사용자가 입력한 아이디는 "+mem_id+" 입니다.");
%>
```

* URL을 보면 get방식으로 넘길수도 있음을 알 수 있다.

## 3단계 : JS : window.onload, JQuery : $\(document\)ready 호출

![1](../../.gitbook/assets/1%20%2841%29.png)

![2](../../.gitbook/assets/2%20%2832%29.png)

![&#xC774;&#xB3D9; &#xD074;&#xB9AD;&#xC2DC;](../../.gitbook/assets/3%20%2824%29.png)

```markup
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script type="text/javascript" src="http://cdnjs.cloudflare.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
<script type="text/javascript">
	var g_id;//멤버변수
	var g_id2//멤버변수
	function move(p_id){
		alert("p_id : "+p_id+", g_id : "+g_id+", g_id2 : "+g_id2);
		//location.href="move.jsp?mem_id= "+p_id;
	}
</script>
</head>
<body>
<script type="text/javascript">
	window.onload = function(){
		g_id = document.getElementById("mem_id").value;
		alert("g_id : "+g_id);
	};
	$(document).ready(function(){
		g_id2 = $("#mem_id").val();
		alert("g_id2 : "+g_id2)
	})
</script>
<form name="f">
<input type="text" id="mem_id" name="mem_id" value="haha">
<!-- <a href="move.jsp?mem_id= ">이동</a> -->
<!-- <a href="javascript:move(document.f.mem_id.value)">이동</a> -->
<!-- window 브라우저로 고정해야 window로 해석한다. -->
<a href="javascript:window.location.href='move.jsp'">이동</a>
</form>
</body>
</html>
```

### 

