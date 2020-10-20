# JS 문자열이벤트

## clickEvent.html

```markup
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
    <h1>A - <span id="count_a">0</span></h1>
    <h1>B - <span id="count_b">0</span></h1>
	<button id="a">A</button>
	<button id="b">B</button>
	<script type="text/javascript">
		var btnA = document.getElementById('a');
		var btnB = document.getElementById('b');
		var countA = document.getElementById('count_a');
		var countB = document.getElementById('count_b');
		//이벤트 연결
		btnA.onclick = function(){
			countA.innerHTML = Number(countA.innerHTML)+1;
		}
		btnB.onclick = function(){
			countB.innerHTML = Number(countB.innerHTML)-1;
		}
	</script>
</body>
</html>
```

## 실행

![](../../.gitbook/assets/3%20%2818%29.png)

* A버튼을 누르면 A - 숫자가 증가, B버튼을 누르면 B - 숫자가 감소한다.

