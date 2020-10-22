# JQuery each문 활용 - arrayTest

### arrayTest.html

![](../../.gitbook/assets/7%20%286%29.png)

```markup
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
<script type="text/javascript">
	
</script>
</head>
<body>
<script type="text/javascript">
	$(document).ready(function(){//JQuery가 import되어있어야 $를 사용할 수 있다.
		//배열선언하기
		var array = [
			{name: 'Naver', link: 'http://naver.com'}
			,{name: 'Daum', link: 'http://daum.net'}
			,{name: 'Hanbit', link: 'http://hanb.co.kr'}
		];
	
		//실행문, 구현부
		$.each(array, function(index,item){//h1요소 내부, this = h1접근
			var output='';
			output +='<a href="'+item.link+'">';//append같은건가?
			output +='<h1>'+item.name+'</h1>';
			output +='</a>'
			//화면에 붙이기 
			document.body.innerHTML += output;
		});/////end of each문
	});/////////end of read
</script>
<a href="javascript:window.history.back()">이전페이지</a>
<a href="javascript:window.history.go(1)">다음페이지</a>
</body>
</html>
```

* 각 name을 누르면 해당 페이지로 이동한다.

