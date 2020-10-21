# receiveMessage.html - 같은name 태그

## receiveMessage.html

```markup
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>보낸쪽지함</title>
<!-- 함수선언은 head에 해야한다. -->
<script type="text/javascript">
	function checkTest(size){
		alert(document.f_receive.checkAll.checked);//단위테스트
		//전체체크박스 선택
		if(document.f_receive.checkAll.checked==1){//checkbox는 체크되면 1(true), 해제되면 0(false)
			for(var i=0;i<size;i++){
				document.f_receive.from[i].checked=1;
			}
		}
		//전체체크박스 해제
		else if(document.f_receive.checkAll.checked==0){
			for(var i=0;i<size;i++){
				document.f_receive.from[i].checked=0;
			}
		}
	}
</script>
</head>
<body>
<center>
<!-- name에 접근할때는 <form>태그를 거쳐서, id는 바로 접근가능 -->
<form name="f_receive">
	<table border="1" borderColor="orange" width="500px" height="300px">
		<thead>받은쪽지함</thead>
		<tr height="100px">
		    <!-- checkbox함수의 파라미터로 넘기는 값은 DB연동없이 test만 할 것이므로 상수를 이욯 -->
			<th height="50px"><input type="checkbox" name="checkAll" onclick="checkTest(3)"></th>
			<th>순번</th>
			<th>보낸이</th>
		</tr>
		<tr align="center">
			<td><input type="checkbox" name="from"></td>
			<td>1</td>
			<td>뽀로로</td>
		</tr>
		<tr align="center">
			<td><input type="checkbox" name="from"></td>
			<td>2</td>
			<td>펭수</td>
		</tr>
		<tr align="center">
			<td><input type="checkbox" name="from"></td>
			<td>3</td>
			<td>핑구</td>
		</tr>
	</table>
</form>
</center>
</body>
</html>
```

## receiveMessage\_2.html

```markup
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>보낸쪽지함</title>
<!-- 함수선언은 head에 해야한다. -->
<script type="text/javascript">
	function checkTest(size){
		//전체체크박스 선택
		if(document.f_receive.checkAll.checked==1){//checkbox는 체크되면 1(true), 해제되면 0(false)
			if(size==1){
				document.f_receive.checkAll.checked==1;
				document.f_receive.from.checked==1;
			}else {
				for(var i=0;i<size;i++){
					document.f_receive.checkAll.checked==1;
					document.f_receive.from[i].checked=1;
				}
			}
		}
		//전체체크박스 해제
		else if(document.f_receive.checkAll.checked==0){
			if(size==1){
				document.f_receive.checkAll.checked==0;
				document.f_receive.from.checked==0;
			}else {
				for(var i=0;i<size;i++){
					document.f_receive.from[i].checked=0;
				}
			}
		}
	}
</script>
</head>
<body>
<center>
<!-- name에 접근할때는 <form>태그를 거쳐서, id는 바로 접근가능 -->
<form name="f_receive">
	<table border="1" borderColor="orange" width="500px" height="300px">
		<thead>받은쪽지함</thead>
		<tr height="100px">
		    <!-- checkbox함수의 파라미터로 넘기는 값은 DB연동없이 test만 할 것이므로 상수를 이욯 -->
			<th height="50px"><input type="checkbox" name="checkAll" onclick="checkTest(3)"></th>
			<th>순번</th>
			<th>보낸이</th>
		</tr>
		<tr align="center">
			<td><input type="checkbox" name="from"></td>
			<td>1</td>
			<td>뽀로로</td>
		</tr>
		<tr align="center">
			<td><input type="checkbox" name="from"></td>
			<td>2</td>
			<td>펭수</td>
		</tr>
		<tr align="center">
			<td><input type="checkbox" name="from"></td>
			<td>3</td>
			<td>핑구</td>
		</tr>
	</table>
</form>
</center>
</body>
</html>
```

## 실행

* 둘다 실행 화면은 같다.

![1](../../.gitbook/assets/1%20%2836%29.png)

![2](../../.gitbook/assets/2%20%2826%29.png)

![3](../../.gitbook/assets/3%20%2822%29.png)

![4](../../.gitbook/assets/4%20%2817%29.png)

![5](../../.gitbook/assets/5%20%2812%29.png)

