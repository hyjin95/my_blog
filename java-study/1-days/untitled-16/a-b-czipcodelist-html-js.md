# A,B,CzipcodeList - HTML, JS

## 공통코드 

```markup
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>부서관리-typeB[태그+JS 코딩연습예제]</title>
	<link rel="stylesheet" type="text/css" href="https://www.jeasyui.com/easyui/themes/default/easyui.css">
    <link rel="stylesheet" type="text/css" href="https://www.jeasyui.com/easyui/themes/icon.css">
    <link rel="stylesheet" type="text/css" href="https://www.jeasyui.com/easyui/themes/color.css">
    <link rel="stylesheet" type="text/css" href="https://www.jeasyui.com/easyui/demo/demo.css">
    <script type="text/javascript" src="https://www.jeasyui.com/easyui/jquery.min.js"></script>
    <script type="text/javascript" src="https://www.jeasyui.com/easyui/jquery.easyui.min.js"></script>
```

* import

## html 태그 중심 구현

![&#xC870;&#xD68C;&#xBC84;&#xD2BC;&#xC744; &#xB20C;&#xB7EC;&#xC57C; data&#xAC00; &#xCD9C;&#xB825;&#xB41C;&#xB2E4;.](../../../.gitbook/assets/6%20%2812%29.png)

```markup
<head>
<meta charset="UTF-8">
<title>우편번호 검색기-typeA[태그중심의 코딩연습예제]</title>
	    <script type="text/javascript">
    	function getTest(){
    		$("#dg_test").datagrid({				
				url:"../getTest.jsp"				
    		});
    	}
    	function add(){
    		$("#display").text("add호출");
    	}
		function edit(){
    		$("#display").text("edit호출");
    		
    	}
		function remove(){
    		$("#display").text("remove호출");
    		
    	}
    </script>
</head>
<body>
<div id="display"></div>
<hr>
<!-- <table class="easyui-datagrid" data-options="url:'test.json'"><!-- test.json:정적 --> 
	<table id="dg_test" title="테스트" toolbar="#tb_test" class="easyui-datagrid"><!--test.jsp:동적 -->
	    <thead>
	        <tr>
	            <th data-options="field:'code',width:100">Code</th>
	            <th data-options="field:'name',width:100">이름</th>
	        </tr>
	    </thead>
	</table>
	<div id="tb_test">
	        <a href="javascript:void(0)" class="easyui-linkbutton" iconCls="icon-search" plain="true" onclick="getTest()">조회</a>
	        <a href="javascript:void(0)" class="easyui-linkbutton" iconCls="icon-add" plain="true" onclick="add()">입력</a>
	        <a href="javascript:void(0)" class="easyui-linkbutton" iconCls="icon-edit" plain="true" onclick="edit()">수정</a>
	        <a href="javascript:void(0)" class="easyui-linkbutton" iconCls="icon-remove" plain="true" onclick="remove()">삭제</a>
	</div>
</body>
</html>
```

## JS중심 구현

![](../../../.gitbook/assets/7%20%288%29.png)

```markup
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>우편번호 검색기-typeC[JS중심의 코딩연습예제]</title>
</head>
<body>
<script type="text/javascript">
	$(document).ready(function(){
		//dg_test는 아이디 값이고 datagrid는 테이블을 지원하는 객체 생성자를 호출하는 문장이다.
		//좌중과호 우중괄호 안에 datagrid안에 처리하고싶은 기능들을 담을 수 있다.
		$("#dg_test").datagrid({
			title:"타입3번-스크립트만으로 화면 그려보기"
			,url:'../getTest.jsp'
		    ,toolbar:'#tb'
			,columns:[[
		        {field:'code',title:'Code',width:100},
		        {field:'name',title:'Name',width:100},
		        {field:'price',title:'Price',width:100,align:'right'}
		    ]]
		});
	});//body에 작성하면 돔트리 구성이 끝나면(id로접근가능) 무조건 실행된다.
</script>
<table id="dg_test" class="easyui-datagrid">	
</table>
<div id="tb">
		<a href="javascript:void(0)" class="easyui-linkbutton" iconCls="icon-search" plain="true" onclick="getDeptList()">조회</a>
		<a href="javascript:void(0)" class="easyui-linkbutton" iconCls="icon-add" plain="true" onclick="addDept()">입력</a>
		<a href="javascript:void(0)" class="easyui-linkbutton" iconCls="icon-edit" plain="true" onclick="editDept()">수정</a>
		<a href="javascript:void(0)" class="easyui-linkbutton" iconCls="icon-remove" plain="true" onclick="removeDept()">삭제</a>
</div>
</body>
</html>
```

## html+JS구현

![](../../../.gitbook/assets/1%20%2849%29.png)

```markup
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>부서관리-typeB[태그+JS 코딩연습예제]</title>
	<link rel="stylesheet" type="text/css" href="https://www.jeasyui.com/easyui/themes/default/easyui.css">
    <link rel="stylesheet" type="text/css" href="https://www.jeasyui.com/easyui/themes/icon.css">
    <link rel="stylesheet" type="text/css" href="https://www.jeasyui.com/easyui/themes/color.css">
    <link rel="stylesheet" type="text/css" href="https://www.jeasyui.com/easyui/demo/demo.css">
    <script type="text/javascript" src="https://www.jeasyui.com/easyui/jquery.min.js"></script>
    <script type="text/javascript" src="https://www.jeasyui.com/easyui/jquery.easyui.min.js"></script>
    <script type="text/javascript">
    var g_deptno=0;//선언 delete에 ?를 찾기 위한 변수
    //조회
    function getTest(){
		$("#dg_test").datagrid({				
			url:"../dept.json"				
		});
	}
    //입력
	function add(){
		$("#display").text("add호출");
	}
    //수정
	function edit(){
		$("#display").text("edit호출");
		
	}
    //삭제
	function remove(){
		$("#display").de
		//delete from dept where deptno=?
	}
    </script>
</head>
<body>
<script type="text/javascript">
	$(document).ready(function(){//ready가 있으면 순서에 상관없지만 ready 없이 body에 구현하려면 해당 id의 선언,생성 뒤에 와야한다.
		//$("#pp_dept").pagination({
		//	total:20
		//	,pageSize:10
		//	,pageList:[3,7,10]
		//});
		$("#dg_dept").datagrid({
			title:"부서관리"
			,url:"../dept.json"
			,fitColumns:true
			,singleSelect:true
			,toolbar:"#tb_dept"
			,pagination:"#pp_dept"
			//function(index=몇번째, field=컬럼명, value=값)
			,onSelect: function(index,row){
				//var row2 = $("#dg_dept").datagrid("getSelected");//선택된 row를 반환하는 getSelected 단위테스트
				//g_deptno = row2.DEPTNO;//단위테스트
				//alert(index+", "+row+", " +row.DEPTNO+", "+row2.DNAME);//단위테스트
				g_deptno = row.DEPTNO;
			}///////////////end of onSelect
			,onDblClickCell: function(index,field,value){
				//alert(value);//단위테스트
				if("LOC"==field){
					location.href="../deptDelete.jsp?deptno="+g_deptno//페이지 이동,URL이 바뀐다.
				}
			}
		});/////////////////end of datagrid
	});/////////////////////end of ready-스캔완료
</script>
<table id="dg_dept" class="easyui-datagrid" style="width:700px; height:250px">
    <thead>
        <tr>
            <th data-options="field:'DEPTNO',width:100">부서번호</th>
            <th data-options="field:'DNAME',width:100">부서명</th>
            <th data-options="field:'LOC',width:100,align:'right'">지역</th>
        </tr>
    </thead>
</table>
<div id="tb_dept">
		<a href="javascript:void(0)" class="easyui-linkbutton" iconCls="icon-search" plain="true" onclick="getDeptList()">조회</a>
		<a href="javascript:void(0)" class="easyui-linkbutton" iconCls="icon-add" plain="true" onclick="addDept()">입력</a>
		<a href="javascript:void(0)" class="easyui-linkbutton" iconCls="icon-edit" plain="true" onclick="editDept()">수정</a>
		<a href="javascript:void(0)" class="easyui-linkbutton" iconCls="icon-remove" plain="true" onclick="removeDept()">삭제</a>
</div>
<div id="pp_dept">
</div>
</div>
</body>
</html>
```

