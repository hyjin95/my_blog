# onchange 이벤트 핸들러

## onchange.html - 1단

![&#xAE30;&#xBCF8; &#xBE0C;&#xB77C;&#xC6B0;&#xC800;](../../.gitbook/assets/1%20%2833%29.png)

![check &#xD568;&#xC218; &#xC774;&#xBCA4;&#xD2B8; - &#xB2E4;&#xC12F;&#xAE00;&#xC790; &#xC785;&#xB825;&#xC2DC;](../../.gitbook/assets/2%20%2828%29.png)

![check2 &#xD568;&#xC218; &#xC774;&#xBCA4;&#xD2B8;](../../.gitbook/assets/3%20%2821%29.png)

```markup
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script type="text/javascript">
	function check(u_name){
		if(u_name.value.length > 4){
			alert("4자만 입력할 수 있습니다.")
			u_name.value="";
		}
	}
	function check2(sel){
		document.getElementById("d_result").innerHTML = sel.value;	
	}
</script>
</head>
<body>
<!-- this=해당 태그에 대한 -->
<input type="text" size="20" onchange="check(this)">
<select id="menu" name="menu" onchange="check2(this)">
	<option value="잔치국수">잔치국수</option>
	<option value="김치볶음밥">김치볶음밥</option>
	<option value="닭가슴살">닭가슴살</option>
	<option value="닭고기">닭고기</option>
	<option value="닭찌찌">닭찌찌</option>
</select>
<!-- innerHTML을 넣어 출력할 div생성 -->
<div id="d_result"></div>
</body>
</html>
```

## onchange - 2단계

![&#xAE30;&#xBCF8; &#xBE0C;&#xB77C;&#xC6B0;&#xC800;](../../.gitbook/assets/1%20%2837%29.png)

![check2 -&amp;gt; check&#xD568;&#xC218; &#xC774;&#xBCA4;&#xD2B8;](../../.gitbook/assets/2%20%2826%29.png)

```markup
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script type="text/javascript">
	function check(u_name){
		if(u_name.value.length > 4){
			alert("4자만 입력할 수 있습니다.")
			u_name.value="";
		}
	}
	function check2(sel){
		document.getElementById("d_result").innerHTML = sel.value;	
		document.getElementById("t_menu").value = sel.value;	
	}
</script>
</head>
<!-- xml이라
<% 
 String menu = request.getParameter("menu"; //abcde가 넘어간다. value가)
%> -->
<body> 
<!-- this=해당 태그에 대한 -->
<input type="text" id="t_menu" size="20" value="저녁" onchange="check(this)">
<select id="menu" name="menu" onchange="check2(this)">
	<option value="a">잔치국수</option> <!-- 태그사이의 문자열은 화면에 보여지는 부분이다. value랑은 다름 -->
	<option value="b">김치볶음밥</option>
	<option value="c">닭가슴살</option>
	<option value="d">닭고기</option>
	<option value="e">닭찌찌</option>
</select>
<!-- innerHTML을 넣어 출력할 div생성 -->
<div id="d_result"></div>
</body>
</html>
```

## 

