# 여러 이미지에 CSS 반영 - cssStep3

![&#xBE0C;&#xB77C;&#xC6B0;&#xC800;](../../.gitbook/assets/1%20%2842%29.png)

```markup
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>문자열 속성 선택자</title>
<style type="text/css">
	li.select {color:orange;}
	/*img태그 중에서 src 속성값이 jpg로 끝나는태그의 보더 속성을 적용할 수 있는 선택자.*/
	img[src$=jpg] {border: 5px solid green;}
	img[src$=png] {border: 5px solid yellow;}
	li > a:first-child {color:red;}
</style>
</head>
<body>
	<img src="../images/react.png" width="200px" height="250px"/>
	<img src="../images/1.jpg" width="200px" height="250px"/>
	<img src="http://192.168.0.187:9000/images/python.png" width="200px" height="250px"/>
	<ul>
		<li class="select"><a href="#">Java</a></li><!-- #:앵커태그 -->
		<li><a href="#">로그인</a></li>
		<!-- 앵커태그에 href속성을 사용하면 페이지 이동이 get방식으로 일어난다. -->
		<!-- get방식은 인터셉트가 일어나 서버까지 전달이 이루어지지 않는다.
			 브라우저를 새로 열어야 하므로 session아이디(서버측[캐시메모리에저장]에서 제공)가 달라진다.
			 세션아이디는 로컬 컴퓨터 쿠키[Text저장:보안에취약]에 저장된다. -->
		<li class="select"><a href="./memberShip.html">회원<font size="16">가입</font></a></li><!-- get방식 -->
	</ul>
</body>
</html>
```

