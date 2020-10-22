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

