# mime타입 분석

## ranking.jsp - html

![&#xBE0C;&#xB77C;&#xC6B0;&#xC800;](../../.gitbook/assets/1%20%2834%29.png)

![&#xAC1C;&#xBC1C;&#xC790;&#xB3C4;&#xAD6C;](../../.gitbook/assets/2%20%2824%29.png)

```markup
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<% String title = "자바서버페이지"; %>
<table>
	<tr><td><%=title %></td></tr>
	<tr><td><% out.print(title); %></td></tr>
</table>
</body>
</html>
```

## xmlDataSet.jsp - xml

![&#xBE0C;&#xB77C;&#xC6B0;&#xC800;](../../.gitbook/assets/3%20%2820%29.png)

```markup
<%@ page import="java.util.List"%>
<%@ page language="java" contentType="text/xml; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ page trimDirectiveWhitespaces="true"%>
<?xml version="1.0" encoding="UTF-8"?>
<records>
	<record>
		<deptno>10</deptno>
		<dname>영업부</dname>
		<loc>서울</loc>
	</record>
	<record>
		<deptno>20</deptno>
		<dname>개발부</dname>
		<loc>제주도</loc>
	</record>
</records>
```

