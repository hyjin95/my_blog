# JAVA변수와 JS변수 사용 : TimeLine

## JAVA변수를 사용하면

### 화면

![Java&#xBCC0;&#xC218;](../../../.gitbook/assets/java-%20%281%29.png)

### 소스보기

![Java&#xBCC0;&#xC218; &#xC18C;&#xC2A4;&#xBCF4;&#xAE30;](../../../.gitbook/assets/java-.png)

* 소스에 변수로 작성했던 14, 17번 라인이 결정되어 있는 것을 볼 수 있다.
* 브라우저가 다운로드할때, 값과 태그를 같이 다운로드 하는 것을 알 수 있다.
* 변수들이 먼저 결정된다.

### 코드 : 20201116Ver5

```markup
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
   String v_name="이순신";
   String v_gender="남자";
%>    
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script type="text/javascript" src="https://www.jeasyui.com/easyui/jquery.min.js"></script>
</head>
<body>
<script type="text/javascript">
   $(document).ready(function(){
      document.write("<table width='350px' border='1' borderColor='green'>");
      document.write("<tr>");
      document.write("<td>");
      document.write("<%=v_name%>");
      document.write("</td>");
      document.write("<td>");
      document.write("<%=v_gender%>");
      document.write("</td>");
      document.write("</tr>");
      document.write("</table>");      
   });
</script>
</body>
</html>
```

## JS변수를 사용하면

### 화면

![JS&#xBCC0;&#xC218;](../../../.gitbook/assets/js-%20%281%29.png)

### 소스보기

![](../../../.gitbook/assets/js-.png)

* 18번, 21번에서 작성한 변수가 결정되어있지 않음을 알 수 있다.

### 코드 : 20201116Ver4

```markup
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script type="text/javascript" src="https://www.jeasyui.com/easyui/jquery.min.js"></script>
<script type="text/javascript">
   var v_name="이순신";
   var v_gender="남자";
</script>
</head>
<body>
<script type="text/javascript">
   $(document).ready(function(){
      document.write("<table width='350px' border='1' borderColor='green'>");
      document.write("<tr>");
      document.write("<td>");
      document.write(v_name);//JS변수, java변수아님
      document.write("</td>");
      document.write("<td>");
      document.write(v_gender);
      document.write("</td>");
      document.write("</tr>");
      document.write("</table>");      
   });
</script>
</body>
</html>
```

## 응용

### 화면 : 기본

![document.write](../../../.gitbook/assets/document.write.png)

### 화면 : onclick

![document.write = &#xB36E;&#xC5B4;&#xC4F0;&#xAE30;](../../../.gitbook/assets/document.write2.png)

### 코드 : 20201116Ver6

```markup
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
   String v_name="이순신";
   String v_gender="남자";
%>    
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script type="text/javascript" src="https://www.jeasyui.com/easyui/jquery.min.js"></script>
<script type="text/javascript">
	var g_dname="영업부"
	function refresh(){
		document.write("<%=v_name%>"+"님 <b>안녕</b>하세요.");
		document.write("<br>");
		document.write(g_dname);
		document.write("<br>");
		document.write(v_dname);
	}
</script>
</head>
<body>
<input type="button" value="전송" onclick="refresh()">
<script type="text/javascript">
	  v_dname="총무부"
	  document.write("<table width='350px' border='1' borderColor='green'>");
      document.write("<tr>");
      document.write("<td>");
      document.write("<%=v_name%>");
      document.write("</td>");
      document.write("<td>");
      document.write("<%=v_gender%>");
      document.write("</td>");
      document.write("</tr>");
      document.write("</table>");
</script>

</body>
</html>
```

* 4,5 번은 지역변수, 14번은 멤버변수, 27번은 지역변수의 성격을 갖는다.
* 가장먼저 결정되는 변수 값은 몇번인가요? - 4,5번
* 29-37번의 document.write는 유지되지 않고 사라진다. - append의 성격이 아니라 setText의 성격 - 기존 정보가 사라진다. - 버튼을 누르면 기존 내용이 사라져버리고 16-20번 코드가 화면에 덮어씌워진다.

