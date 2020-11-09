# HangulConversion.java : UTF-8변환, get, post

## input

```markup
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>전송방식에 대한 한글 테스트-server.xml에 urf-8설정</title>
	<link rel="stylesheet" type="text/css" href="https://www.jeasyui.com/easyui/themes/default/easyui.css">
    <link rel="stylesheet" type="text/css" href="https://www.jeasyui.com/easyui/themes/icon.css">
    <link rel="stylesheet" type="text/css" href="https://www.jeasyui.com/easyui/themes/color.css">
    <link rel="stylesheet" type="text/css" href="https://www.jeasyui.com/easyui/demo/demo.css">
    <script type="text/javascript" src="https://www.jeasyui.com/easyui/jquery.min.js"></script>
    <script type="text/javascript" src="https://www.jeasyui.com/easyui/jquery.easyui.min.js"></script>
</head>
<body>
<form id="f" method="get" action="inputAction.jsp">
	<input id="tb" name="name" class="easyui-textbox">
	<button type="submit" value="전송-get">전송-get</button>
</form>

<form id="f2" method="post" action="inputAction.jsp">
	<input id="tb2" name="name" class="easyui-textbox">
	<button type="submit" value="전송-post">전송-post</button>
</form>
</body>
</html>
```

## inputAction.jsp

```java
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ page import="com.util.HangulConversion" %>
<%
	out.print("nomal : "+request.getParameter("name"));
	out.print("<br>");
	out.print("toKor : "+HangulConversion.toKor(request.getParameter("name")));
	out.print("<br>");
	out.print("toUTF : "+HangulConversion.toUTF(request.getParameter("name")));
	out.print("<br>");
%>
```

### HangulConversion.java

```java
package com.util;
//한글 처리
public class HangulConversion {
		
	public static String toKor(String en) {
		if(en == null) return null;
		try {
			return new String(en.getBytes("8859_1"),"euc-kr");
		} catch (Exception e) {
			return en;
		}
	}////end of toKor
	
	public static String toUTF(String en) {
		if(en == null) return null;
		try {
			return new String(en.getBytes("8859_1"),"utf-8");
		} catch (Exception e) {
			return en;
		}
	}////end of toKor
}
```

## 실행

![get &#xBC29;&#xC2DD;](../../../.gitbook/assets/3%20%2838%29.png)

![&#xACB0;&#xACFC;](../../../.gitbook/assets/4%20%2832%29.png)

![post &#xBC29;&#xC2DD;](../../../.gitbook/assets/5%20%2823%29.png)

![&#xACB0;&#xACFC;](../../../.gitbook/assets/6%20%2817%29.png)

