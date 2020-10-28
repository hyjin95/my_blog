# TextboxTest, DeptView - textbox, datagrid

## 1단계 : Textbox API test하기

### TextboxTest

```markup
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>textboxText.html</title>
<!-- 공통코드 추가[css, jquery.js, easyui.js] -->
	<link rel="stylesheet" type="text/css" href="https://www.jeasyui.com/easyui/themes/default/easyui.css">
    <link rel="stylesheet" type="text/css" href="https://www.jeasyui.com/easyui/themes/icon.css">
    <link rel="stylesheet" type="text/css" href="https://www.jeasyui.com/easyui/themes/color.css">
    <link rel="stylesheet" type="text/css" href="https://www.jeasyui.com/easyui/demo/demo.css">
    <script type="text/javascript" src="https://www.jeasyui.com/easyui/jquery.min.js"></script>
    <script type="text/javascript" src="https://www.jeasyui.com/easyui/jquery.easyui.min.js"></script>
    <style type="text/css">
    	#d_msg{
    		border:3px solid pink;
    		width: 200px;
    		height: 40px;
    	}
    </style>
</head>
<body>
	<script type="text/javascript">
   		$(document).ready(function(){
	   		$("#tb").textbox('setValue', '테스트');
   		});
    </script>
    
<!-- <div>는 공간분할용도로 사용하거나 msg를 출력할떄 사용하거나, AJAX에서 화면을 추가할때에도 사용한다. -->
	<div id="d_msg"></div>
	
<!-- 표준이 아닌 easyui를 사용했을 때 추가되는 태그들을 관찰해보자 -->
	<div style="margin-bottom:20px">
            <input value="kim@bee.com" id="tb"
            	   class="easyui-textbox" label="Email:" 
            	   labelPosition="top" data-options="prompt:'Enter a email address...',validType:'email'" 
            	   style="width:100%;">
	</div>
	
   <script type="text/javascript">
   		//var email = $("#tb").textbox("getValue");
   		$("#d_msg").text("div영역 : "+$("#tb").val());
   		//t.textbox('setValue', $(this).val());//1단계-API복사
   		//"#_easyui_textbox_input1".textbox('setValue', $(this).val());//2단계
   		$("#_easyui_textbox_input1").textbox('setValue', '테스트2');//3단계
    </script>
</body>
</html>
```

### 실행

![](../../.gitbook/assets/5%20%2818%29.png)

## 2단계 : datagrid와 textbox 

### deptView

```markup
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>부서관리-시작화면</title>
	<!-- JAVA의 import undefined, 404-->
	<link rel="stylesheet" type="text/css" href="../../themes/default/easyui.css">
    <link rel="stylesheet" type="text/css" href="../../themes/icon.css">
    <link rel="stylesheet" type="text/css" href="../../demo/demo.css">
    <script type="text/javascript" src="../../js/jquery.min.js"></script>
    <script type="text/javascript" src="../../js/jquery.easyui.min.js"></script>
</head>
<body>
<script type="text/javascript">
	/*
		더블쿼테이션과 싱글쿼테이션은 구분하지 않지만 같이 사요할 때에는 바깥쪽이 더블이면 안쪽을 싱글로,
		바깥쪽이 싱글이면 안쪽에는 더블을 사용한다.
		datagrid에 접근할 때에는 반드시 #아이디로 선언 후, 객체생성한다.
	*/
	$(document).ready(function(){
		$("#dg_dept").datagrid({
			url:'http://localhost:9000/EasyUI/datagrid/dept.json'
			//url:'dept.json'
			,onSelect:function(index,row){
				$("#tb_deptno").textbox('setValue', row.DEPTNO);
				$("#tb_dname").textbox('setValue', row.DNAME);
				$("#tb_loc").textbox('setValue', row.LOC);
			}
		});
	});
</script>

<table id="dg_dept" class="easyui-datagrid" style="width:400px;height:250px"
        data-options="fitColumns:true,singleSelect:true">
    <thead>
        <tr>
            <th data-options="field:'DEPTNO',width:100,align:'center'">부서번호</th>
            <th data-options="field:'DNAME',width:100,align:'center'">부서명</th>
            <th data-options="field:'LOC',width:100,align:'center'">지역</th>
        </tr>
    </thead>
</table>
<hr>
<input id="tb_deptno"
       class="easyui-textbox" label="부서번호 : " 
       style="width:150px;">
<input id="tb_dname"
       class="easyui-textbox" label="부서명 : " 
       style="width:200px;">
<input id="tb_loc"
       class="easyui-textbox" label="지역 : " 
       style="width:200px;">
</body>

```

### 실행

![](../../.gitbook/assets/6%20%2813%29.png)



