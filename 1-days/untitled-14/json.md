# JSON을 활용해 요청 페이지에 응답받기

## jsonShopInfo.jsp

![&#xBE0C;&#xB77C;&#xC6B0;&#xC800;](../../.gitbook/assets/1%20%2846%29.png)

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

* JSP파일안에 data 담아놓기 배열형태로 저장되지만 외부에서 꺼내면 Object형태의 한 덩어리로 꺼내진다.
* 사용할 java의 함수들과 Gson을 import해야한다.
* 25번 구문으로 브라우저에 출력해보기

## jsonShopTest.html

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

* 12번 : ajax를 활용해 요청화면과 응답화면이 똑같도록 한다.
* 15,16번 : 받아온 정보를 찍어보면 Object덩어리로 들어온 것을 알 수 있다.
* 19번 : 덩어리를 조각 내주는 JSON의 stringity\( \)함수를 사용해 분리한다.
* 22번 : 분리된 조각들을 JSON의 parse함수를 이용해 배열인 객체로 접근할 수 있도록 해준다.
* 23번 : 이때 length를 찍으면 3이 나오는 것을 알수 있다.
* 24,26번 : for문을 돌려 배열안의 값을 출력해본다.

## 실행

![data&#xC5D0; &#xB2F4;&#xAE34;&#xAC83;&#xC740; Object &#xB369;&#xC5B4;&#xB9AC;](../../.gitbook/assets/2%20%2834%29.png)

![Stringify&#xB85C; &#xC798;&#xB77C; Json.parse&#xB85C; &#xBCC0;&#xD658;&#xD574;&#xBCF8; &#xBC30;&#xC5F4;&#xC758; &#xBC29;&#xC758; &#xAC2F;&#xC218;&#xB294; 3&#xAC1C;](../../.gitbook/assets/3%20%2827%29.png)

![25&#xBC88; &#xAD6C;&#xBB38;, &#xC21C;&#xC11C;&#xB300;&#xB85C; &#xC774;&#xB807;&#xAC8C; &#xCD9C;&#xB825;&#xB41C;&#xB2E4;.](../../.gitbook/assets/4%20%2822%29.png)

![16&#xBC88; &#xAD6C;&#xBB38;, &#xB9C8;&#xC9C0;&#xB9C9;&#xC5D0; &#xCC0D;&#xD788;&#xB294; &#xAC12;](../../.gitbook/assets/5%20%2816%29.png)

