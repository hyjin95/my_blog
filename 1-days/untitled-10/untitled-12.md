# t\_guguDan.html

## guguDan.html

{% page-ref page="../untitled-9/untitled-1.md" %}

## t\_guguDan.html

```markup
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script type="text/javascript">
	function account(p_start, p_end){
		document.write("<center><h1>구구단</h1><table width='90%'><tr>");
		//단수 처리하기
		for(let i = p_start; i <= p_end; i++) { 
			document.write("<td bgcolor='green' width='80px' height='50px' align='center'>",i,"단","</td>");
			if(i==p_end) document.write("</tr><tr>");
		}
		//계산 처리하기
		for(var k=p_start;k<=p_end;k++){
			document.write("<td bgcolor='yellow' width='80px' height='50' align='center'><font color='green'><b>");
			for(var j=1;j<=9;j++){
				document.write(k," * ",j," = ",k*j,"<br>");
			}
			document.write("</b><font></td>");
			}
		//바닥 처리하기 - 닫는태그 정리
		document.write("</tr></table>");	
		document.write("</center>");
		document.write("<br>");
		document.write("시작단 : "+p_start);//전부 유효수치 정수이다.
		document.write("<br>");
		document.write("끝    단 : "+p_end);
	}
</script>
</head>
<body>
<script type="text/javascript">
	document.write("<h1>for문을 활용한 구구단 출력</h1>");
</script>
<form name="f_guguDan" id="f_guguDan">
	시작단 : <input type="text" name="txtStart"><br><!-- 인라인 요소이므로 반드시 <br>붙이기 -->
	끝    단 : <input type="text" name="txtEnd"><br>
	<button id="btn_print" 
	onClick="javascript:account(document.f_guguDan.txtStart.value, document.f_guguDan.txtEnd.value)">구구단 출력하기</button><!-- click속성에 함수를 사용할 수 있다. -->
</form>
</body>
</html>
```

## 실행

![](../../.gitbook/assets/2%20%2822%29.png)

