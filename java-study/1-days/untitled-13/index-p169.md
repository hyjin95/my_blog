# JQuery만으로 배열에 index로 스타일 부여 - p169

### p169.html

![](../../../.gitbook/assets/3%20%2826%29.png)

```markup
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<!-- import 네트워트를 통해 서버의 jquery연결하기 정상적으로 찾았다면 브라우저에서 status 200이 떨어질 것이다. -->
<!-- 이 코드가 없으면 12번 라인의 alert도 실행되지 않고 개발자도구를 확인하면 오류를 발견할 수 있다. -->
<script type="text/javascript" src="http://192.168.0.187:9000/js/jquery-3.5.1.min.js"></script>

<style type="text/css">
	.class0 {background:green;}
	.class1 {background:orange;}
	.class2 {background:gray;}
</style>
</head>

<body>
<!-- jquery가 지원하지 않는 API는 표준 스크립트를 사용해야 하므로 선언 수 사용을 원칙으로 하고,
 	 순서를 변경할 때에는 반드시 dom스캔 먼저 확인후에 처리하는 것을 원칙으로 해야한다.
 	 jquery는 돔 구성이 완료된 상태에서 동작한다. 10-12번 뒤에 동작한다. -->
<script type="text/javascript">
	//alert("호출이 없어도 실행됨"+$("#txt"));//JQuery는 선언이 뒤에있는 id라도 찾아준다.
	$(document).ready(function(){//DOM구성이 완료 되었을때
		//css에서 선언된 class속성을 add한다.h1이 여러개 이므로 브라우저가 배열로 인식해준다.=index를 사용해 for문 없이도 h1갯수만큼 함수가 진행된다.
		$("h1").addClass(function(index){//index가 0인 h1은 class0번 스타일이, index 1인 h1은 class1번 스타일이, ....
			return "class"+index;
		});
	});
</script>
<h1>header-0</h1>
<h1>header-1</h1>
<h1>header-2</h1>
</body>
</html>
```

