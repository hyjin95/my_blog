# onchange 이벤트 핸들러

## onchange.html

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

## 실행

![&#xAE30;&#xBCF8; &#xBE0C;&#xB77C;&#xC6B0;&#xC800;](../../.gitbook/assets/1%20%2833%29.png)

![check&#xD568;&#xC218; &#xC774;&#xBCA4;&#xD2B8;](../../.gitbook/assets/2%20%2826%29.png)

![check2 &#xD568;&#xC218; &#xC774;&#xBCA4;&#xD2B8;](../../.gitbook/assets/3%20%2821%29.png)

