# onkeyup 이벤트 핸들러

## onkeyupTest.html

```markup
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>onkeyupTest</title>
<script type="text/javascript">
	//자바에서의 public void actionPerformed(AcionEvent e){Obeject obj = e.getSource();}
	//e를 출력하면 그냥 이벤트에 대한 브라우저의 내장객체 Object가 출력된다.
	//JS가 사용하는 메서드들은 브라우저 안에 내장되어있어 인스턴스화 같은 과정 없이 사용하면된다.
	function whenClick(e){
		document.getElementById("d_msg").innerText="클릭 : "+e;
	}
	function whenKeyUp(e){
		alert("keyup : "+e);//한번만 감지하는게 아니라 계속 이벤트를 감지함을 알 수 있음
		document.getElementById("d_msg").innerText="KeyUp : "+e;
	}
</script>
</head>
<body>
    <!-- 정의해둔 whenClick을 onclick시 호출한다. -->
	<h1 onclick="whenClick(event)">click</h1>
	<input type="text" onKeyUp="whenKeyUp(event)">
	<div id="d_msg"></div>
</body>
</html>
```

## 실행

![&#xAE30;&#xBCF8; &#xD654;&#xBA74;](../../../.gitbook/assets/1%20%2831%29.png)

![&#xD0A4;&#xBCF4;&#xB4DC;&#xB97C; &#xB20C;&#xB974;&#xACE0; &#xC190;&#xC744; &#xB5BC;&#xBA74;](../../../.gitbook/assets/2%20%2822%29.png)

![onkeyup &#xC624;&#xBE0C;&#xC81D;&#xD2B8; &#xCD9C;&#xB825;](../../../.gitbook/assets/3%20%2818%29.png)

