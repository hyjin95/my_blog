# hello.html

### hello.html

```markup
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>HelloWorld</title>
<script type="text/javascript">
	function send(){
		//alert("안녕");
		document.f_hello.submit();
	}
</script>
</head>
<body>
<h1>안녕하세요. 웹프로그래밍 시간입니다. 뿡빵뿡</h1>
<button onClick="javascript:send()">오늘 스터디할까?</button>
<form name="f_hello" method ="get" action="helloAction.html">
<input type="text" id="mem_id" name="mem_id">
<input type="text" id="mem_pw" name="mem_pw">
</form>
</body>
</html>
```



