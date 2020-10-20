# domImgTest.html - dom, JS, onload

## donImgTest.html

```markup
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script type="text/javascript">
	function init(){
		//이런방법으로 태그를 생설 할 수 있으므로 JS기반의 UI솔루션이 가능하다.
		var img = document.createElement("img");
		img.setAttribute("src","../images/wood.jpg");
		img.setAttribute("width", 400);
		document.body.appendChild(img);
		img.setAttribute("height", 250);
		img.setAttribute("border", 5);
		document.getElementById("d_img").innerHTML =
			"<img src='http://192.168.0.187:9000/images/wood.jpg' border='10' width='350px' height='200px' alt='나무그림'/>"
	}
</script>
</head>
<body onload="init()">
<!-- 여기 -->
<!-- <hr> -->
<img src="http://192.168.0.187:9000/images/wood.jpg" width="350px" height="200px" alt="나무그림"/>
<div id = "d_img">d_img는 여기에 붙어요</div>
</body>
</html>
```

* alt 값은 해당 이미지가 없을때 브라우저에 표시되는 값

## 실행

