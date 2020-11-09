# HTML페이지 -&gt; JSP페이지 - inputRead

## inputReadTest.html

```markup
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>회원입니다. -업무담당자가 보는 화면</title>
</head>
<body>
<form id="f_test" name="f_test" action="inputReadAction.jsp"><!--  method="POST" -->
	이름 : <input type="text" id="t_name" name="t_name"><br>
	주소 : <input type="text" id="t_addr" name="t_addr"><br>
	상품명 : <input type="text" id="t_good" name="t_good"><br>
	<input type="submit">
</form>
</body>
</html>
```

### inputReadAction.jsp

```javascript
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
//여기는 JAVA영역
	//request.setCharacterEncoding("UTF-8");//post방식
	String name = request.getParameter("t_name");//id가 아닌 name만 받을 수 있다.
	String addr = request.getParameter("t_addr");
	String good = request.getParameter("t_good");
	out.print("이글자 : "+"<b>"+name+"</b>");
	// = JS : document.write()
%>
<!-- 여기는 HTML영역 -->

<table border="1">
	<tr>
		<td><%out.print(addr); %></td>
	</tr>
</table>
```

## 실행

![inputReadTest.html](../../../.gitbook/assets/4%20%2816%29.png)

![inputReadAction.jsp](../../../.gitbook/assets/5%20%2811%29.png)

