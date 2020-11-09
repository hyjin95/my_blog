# empManager - combobox추가

### empManager - combobox추가

![](../../../.gitbook/assets/5%20%2820%29.png)

```markup
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>사원관리(empManager.html)</title>
<!-- 공통코드 추가[css, jquery.js, easyui.js] -->
	<link rel="stylesheet" type="text/css" href="https://www.jeasyui.com/easyui/themes/default/easyui.css">
    <link rel="stylesheet" type="text/css" href="https://www.jeasyui.com/easyui/themes/icon.css">
    <link rel="stylesheet" type="text/css" href="https://www.jeasyui.com/easyui/themes/color.css">
    <link rel="stylesheet" type="text/css" href="https://www.jeasyui.com/easyui/demo/demo.css">
    <script type="text/javascript" src="https://www.jeasyui.com/easyui/jquery.min.js"></script>
    <script type="text/javascript" src="https://www.jeasyui.com/easyui/jquery.easyui.min.js"></script>
<!-- 공통코드 추가 끝 -->
<script type="text/javascript">
	var v_date;//사용자가 선택한 날짜 정보를 담을 변수 선언
</script>
</head>
<body>
<script type="text/javascript">	
	$(document).ready(function(){
		//화면 컴포넌트 - 5 - combobox1
		$('#cb_ename').combobox({
		    url:'../emp.json'
		   ,valueField:'ENAME'
		   ,textField:'ENAME'
		});
		//화면 컴포넌트 - 6 - combobox1
		$('#cb_job').combobox({
		    url:'../emp.json'
		   ,valueField:'JOB'
		   ,textField:'JOB'
		});
		//화면 컴포넌트 - 7 - combobox1
		$('#cb_deptno').combobox({
		    url:'../emp.json'
		   ,valueField:'DEPTNO'
		   ,textField:'DEPTNO'
		});	
	});
</script>
<!-- 화면그리기 시작 -->
<!-- 테이블태그를 활용해 조건 검색하는 화면을 추가하고 그 아래에 버튼을 배치하시오 -->
<div id="tb_emp">
<table border="0" width="100%">
<!-- 조건 검색 화면 시작 -->
	<tr>
		<td>
			<table>
					<!-- combobox추가 (위치선택 -> 공간확보 -> 코드작성,추가) 이름|job|부서번호 -->
				<tr>
					<td width="300px">
						<label width="100px">검색분류 : </label>
						<input id="cb_ename" class="easyui-combobox" value="사원이름">
					</td>
					<td width="300px">
						<label width="100px">검색분류 : </label>
						<input id="cb_job" class="easyui-combobox" style="width:147px" value="직급">
					</td>
					<td width="300px">
						<label width="100px">검색분류 : </label>
						<input id="cb_deptno" class="easyui-combobox" style="width:130px" value="부서번호">
					</td>
					</td>
				</tr>
			</table>			
		</td>		
	</tr>
<!-- 조건 검색 화면 끝 -->
</table>
</div>
</body>
</html>
```

