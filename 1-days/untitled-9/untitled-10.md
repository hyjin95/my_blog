# Untitled

```markup
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>currentTime.html - 현재시간 - JS와 HTML의 상호작용, 관계, 위치</title>
</head>
<body>
<script type="text/javascript">//mime타입
//여기에서 h1태그에 접근할 수 있을까? 아니요 생성되기 전이므로
	//var clock = document.getElementById("clock");
	//alert(clock);//경고팝업 띄우는 함수, 반환값 없이 메세지만 출력 여기서는 null출력
	//해결할 수 있는 방법이 있다. 윈도우 객체의 onload이벤트를 통해 dom트리 구성이 끝났을 때
	//태그에 접근하도록 하는 것이다.
	window.onload = function(){
		var clock = document.getElementById("clock3");
		alert("onload : "+clock);//여기서는 object메세지
	}/////end of onlad - 브라우저가 dom트리 구성을 마쳤을때 로드된다.
</script>
<h1 ID="clock">현재시간</h1>
<h2 ID="clock3">현재시간</h2>
<h3 ID="clock2">현재시간</h3>
<script type="text/javascript">//mime타입
	var clock2 = document.getElementById("clock");
	setInterval(function(){//함수 구현, 자바의 내부 클래스와 같은 익명함수 
		clock2.innerHTML=new Date().toString();
	}, 1000);
	alert(clock2);//함수호출
</script>

<script type="text/javascript">//mime타입
	var clock3 = document.getElementById("clock2");
	setInterval('timer()', 1000);
	function timer(){//timer=Object이름, 생성자.
		var temp = new Date().toString();
	}
	//clock3.innerText="<font clolor='green'>"+'현재시간 : +"</font>"+timer;text타입이므로 태그를 text로 출력
	clock3.innerHTML = "<font clolor='green'>"+'현재시간 : '+"</font>"+temp;
	clock3.innerHTML = temp.slice(0,24);
	alert(clock3);//함수호출
</script>
</body>
</html>
```

