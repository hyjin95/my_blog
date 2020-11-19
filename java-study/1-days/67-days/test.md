# 비동기 통신 : test

## 비동기 통신 : test

### 화면

![](../../../.gitbook/assets/4%20%2834%29.png)

### 코드 : step.jsp - ajax

```markup
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<!-- 공통코드 시작 -->
<%@ include file="/common/easyUI_common.jsp" %>
<!-- 공통코드 종료 -->
<script type="text/javascript">
	$(document).ready(function(){
		$.ajax({
			 url:"innerStep1.jsp?mem_id=apple&mem_pw=123"//쿼리스트링에 띄어쓰기가들어가면 아스키코드로 잡히기때문에 주의한다.
			,success:function(data){
				//=document.getElementById("#d_result").innerText(data); --표준
				//$("#d_result").text(data);//JQuery
				$("#d_result").html(data);//JQuery
			}
		});
	});
</script>
</head>
<body>
<div id="d_result">여기</div>
</body>
</html>
```

### 코드 : innerStep.jsp

```markup
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
	//자바 코드의 처리 주체는 톰캣서버이다.
	//화면으로 나오기전에 결정된다.
	//이 처리 결과는 text 타입이다.(html,xml,json)

	String mem_id = request.getParameter("mem_id");
	String mem_pw = request.getParameter("mem_pw");
	out.print(mem_id+", "+mem_pw);
%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<font color="pink">비동기 통신</font>으로 가져온 새로운 정보
</body>
</html>
```

