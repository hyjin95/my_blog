# guguDan.html

```markup
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script type="text/javascript">
	function account(p_start, p_end){
		//alert("구구단 출력");
		document.write("구구단 출력 화면 입니다.");
		//여기는 자바스크립트 영역 태그를 사용한다.
		document.write("<br>");
		document.write("시작단 : "+p_start);//전부 유효수치 정수이다.
		document.write("<br>");
		document.write("끝    단 : "+p_end.value);
		//document.f_guguDan.submit();//전송 함수

		for(let i = p_start; i <= p_end.value; i++) { 
			document.write("<br>");
			document.write(i + '단'); 
			document.write("<br>");
			for(let j = 1; j <= 9; j++) { 
				document.write(i + ' * ' + j + ' = ' + (i*j)); 
			document.write("<br>");
			} 
		}
	}
</script>
</head>
<body>
<form name="f_guguDan">
	시작단 : <input type="text" name="txtStart"><br><!-- 인라인 요소이므로 반드시 <br>붙이기 -->
	끝단 : <input type="text" name="txtEnd"><br>
	<button id="btn_print" 
	onClick="javascript:account(document.f_guguDan.txtStart.value, document.f_guguDan.txtEnd)">구구단 출력하기</button><!-- click속성에 함수를 사용할 수 있다. -->
</form>
</body>
</html>
```

