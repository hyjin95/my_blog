# JQuery each문 활용 - classTest

## classTest.html 

![](../../../.gitbook/assets/6%20%2810%29.png)

```markup
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
<style type="text/css">
	.high-light1-0 {background:yellow;}
	.high-light1-1 {background:orange;}
	.high-light1-2 {background:blue;}
	.high-light1-3 {background:green;}
	.high-light1-4 {background:pink;}
</style>
<script type="text/javascript">
	function movePrevious(){
		window.history.back();
	}
	function moveNext(){
		window.history.go(1);
	}
</script>
</head>
<body>
<script type="text/javascript">
	$(document).ready(function(){//JQuery가 import되어있어야 $를 사용할 수 있다.
		//실행문, 구현부
		$('h1').each(function(index,item){//h1요소 내부, this = h1접근
			//alert("index : "+index+", item : "+item);출력 단위테스트
			//$(this).addClass('high-light1-'+index);//가능
			$(item).addClass('high-light1-'+index);//가능
		});/////end of each문
	});/////////end of read
</script>
	<h1>item-0</h1>
	<h1>item-1</h1>
	<h1>item-2</h1>
	<h1>item-3</h1>
	<h1>item-4</h1>
 <!-- <a href="javascript:window.history.back()">이전페이지</a>
 <a href="javascript:window.history.go(1)">다음페이지</a> -->
 <a href="javascript.movePrevious()">이전페이지</a>
 <a href="javascript.moveNext()">다음페이지</a>
</body>
</html>
```

