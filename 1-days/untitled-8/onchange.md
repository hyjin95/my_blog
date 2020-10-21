# onchange 이벤트 핸들러

## onchange.html - 1단

![&#xAE30;&#xBCF8; &#xBE0C;&#xB77C;&#xC6B0;&#xC800;](../../.gitbook/assets/1%20%2834%29.png)

![check &#xD568;&#xC218; &#xC774;&#xBCA4;&#xD2B8; - &#xB2E4;&#xC12F;&#xAE00;&#xC790; &#xC785;&#xB825;&#xC2DC;](../../.gitbook/assets/2%20%2829%29.png)

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

![&#xAE30;&#xBCF8; &#xBE0C;&#xB77C;&#xC6B0;&#xC800;](../../.gitbook/assets/1%20%2838%29.png)

![check2 -&amp;gt; check&#xD568;&#xC218; &#xC774;&#xBCA4;&#xD2B8;](../../.gitbook/assets/2%20%2827%29.png)

```markup
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script type="text/javascript">
	function check(u_name, sel){
		document.getElementById("t_menu").value = sel.value;	
	}
	function check2(sel){//sel=document.getElementById(menu)
		document.getElementById("d_result").innerHTML = sel.value;	
		check("111",sel);//내안의 메서드 재사용
	}
</script>
</head>
<!-- 
<% xml이라면
 String menu = request.getParameter("menu"; //abcde가 넘어간다. value가)
%>-->
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

* innerHTML : body태그 안의 &lt;div&gt;로 innerHTML을 적용할 영역을 지정해  HTML 코드를 삽입 - &lt;div&gt;태그로 영역지정된 곳이 아니라면 사용할 수 없다. - 8번 처럼 value를 사용해서 값을 받는다.
* 12번 파라미터의 111값은 위에서 작성한 함수를 재사용하기위해 넣은 의미 없는 값이다.
* 24-28번에서 의미를 갖는 값, Value로 꺼내지는 값과 화면에 보여지기 위해 작성한 값을 구별해야한다.

## 

