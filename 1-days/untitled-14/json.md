# JSON을 활용해 요청 페이지에 응답받기

```markup
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ page import="java.util.List, java.util.Map, java.util.ArrayList" %>
<%@ page import="java.util.HashMap, com.google.gson.Gson"%>
<%
	//jsonShopInfo.jsp파일 추가
	List<Map<String, Object>> shopList = new ArrayList<>();
	Map<String, Object> info = new HashMap<>();
	info.put("loc", "가산디지털단지역");
	info.put("lat", "37.481540");
	info.put("lng", "126.882476");
	shopList.add(info);
	info = new HashMap<>();
	info.put("loc", "남구로역");
	info.put("lat", "37.486007");
	info.put("lng", "126.887273");
	shopList.add(info);
	info = new HashMap<>();
	info.put("loc", "대림역");
	info.put("lat", "37.493051");
	info.put("lng", "126.897088");
	shopList.add(info);
	Gson g = new Gson();
	String temp = g.toJson(shopList);
	out.print(temp);
%>

```

```markup
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
  <script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
</head>
<body>
	<script type="text/javascript">
		var imsi;
		$(document).ready(function(){
			$.ajax({
				url:'jsonShopInfo.jsp'
				,dataType:'json'
				,success:function(data){
					alert(data);
					$("#d_display").append(data);
					//넘어온 값을 JSON형식으로 처리해줌
					var result = JSON.stringify(data);
					$("#d_display").append(result);
					//그 값을 배열 객체로 접근할 수 있도록 해줌
					var jsonDoc = JSON.parse(result);
					alert(jsonDoc.length);
					for(var i=0;i<jsonDoc.length;i++){
						alert(jsonDoc[i].loc);
						$("#d_display").html("<font color='red'>"+jsonDoc[i].loc+"</font>");
					}
				}
			,error:function(request,status,error){
	              alert("code:"+request.status+"\n"+"message:"+request.responseText+"\n"+"error:"+error);
	             }
			});
		});
	</script>
	<div id="d_display"></div>
</body>
</html>
```

