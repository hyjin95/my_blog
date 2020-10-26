# onsubmit 이벤트 핸들러 - onclick, onload

## submitEvent.html

```markup
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>onsubmit : 서버에 전송하는 이벤트 핸들러</title>
</head>
<body>
<!-- 여기에 붙이면 선언이 먼저 사용이 나중이여야하므로 접근 불가능하다. 돔 구성 마친 이후에 코드가 적용되어야함 
      JS코드를 헤드에 사용하지 않고 body에 사용한다.
          차이점 : head에 있는것은 호출로 실행, body에 있는것은 바로 실행-->
<script type="text/javascript">
	window.onload = function(){
		document.getElementById("f_member").onsubmit = function(){
			//패스워드 확인
			var pw = document.getElementById("pw").value;
			var pw_check = document.getElementById("pw_check").value;
			if(pw==pw_check){
				alert("패스워드 일치");
			}
			else{
				alert("패스워드를 확인하세요.");
				return;
			}			
		};//end of onsubmit EventHandler
	};//////end of 돔 트리가 완성 되었을때
</script>
<form id="f_member">
	<table align="center" border="1" borderColor="red" width="500">
		<tr>
			<th width="150">이름 : </th>
			<td width="350">
			<input type="text" size="12">
			<div id="d_name"></div>
			</td>
		</tr>
		<tr>
			<th width="150">아이디 : </th>
			<td width="350">
			<input type="text" size="12">
			<div id="d_id"></div>
			</td>
		</tr>
		<tr>
			<th width="150">패스워드 : </th>
			<td width="350">
			<input type="text" size="12" id="pw">
			<div id="d_pw"></div>
			</td>
		</tr>
		<tr>
			<th width="150">패스워드 확인 : </th>
			<td width="350">
			<input type="text" size="12" id="pw_check">
			<div id="d_pw_check"></div>
			</td>
		</tr>
		<tr>
			<th width="100%" colspan="2" align="center">
			<button id="btn_add">가입</button>
			<button type="button" onClick="cancel()">취소</button>
			</th>
		</tr>
	</table>
</form>
<!-- 여기에 JS 이벤트를 붙이면 form태그가 끝난 뒤이므로 사용 가능하다. -->
</body>
</html>
```

* 60번에서 취소 버튼에 type=button을 넣어주지 않으면 form submit이 실행되어 취소버튼의 기능을 수행하지 않을 수 있다. 

## 실행

![&#xD328;&#xC2A4;&#xC6CC;&#xB4DC;&#xAC00; &#xC77C;&#xCE58;&#xD558;&#xB294; &#xACBD;&#xC6B0;](../../.gitbook/assets/6%20%288%29.png)

![&#xBE44;&#xBC00;&#xBC88;&#xD638;&#xAC00; &#xC77C;&#xCE58;&#xD558;&#xC9C0; &#xC54A;&#xB294; &#xACBD;&#xC6B0;](../../.gitbook/assets/7%20%284%29.png)

