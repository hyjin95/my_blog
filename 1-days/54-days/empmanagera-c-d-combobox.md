# empManagerA,C,D - combobox심화

## empManagerAtype - html

![](../../.gitbook/assets/5%20%2821%29.png)

```markup
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>사원관리(empManagerAType.html)</title>
<!-- 공통코드 추가 시작 
나중에는 이것들을 include를 사용해서 한 줄 추가로 하면 좋을거 같아요.-->
    <link rel="stylesheet" type="text/css" href="https://www.jeasyui.com/easyui/themes/default/easyui.css">
    <link rel="stylesheet" type="text/css" href="https://www.jeasyui.com/easyui/themes/icon.css">
    <link rel="stylesheet" type="text/css" href="https://www.jeasyui.com/easyui/themes/color.css">
    <link rel="stylesheet" type="text/css" href="https://www.jeasyui.com/easyui/demo/demo.css">
    <script type="text/javascript" src="https://www.jeasyui.com/easyui/jquery.min.js"></script>
    <script type="text/javascript" src="https://www.jeasyui.com/easyui/jquery.easyui.min.js"></script>
    <style type="text/css">
        
       #d_msg {
          border: 5px solid orange;
          width: 700px;
          height: 120px;
       }
    </style>
<!-- 공통코드 추가   끝  -->
</head>
<body>
<!-- 이 페이지에 대한 에러 메시지나 힌트문을 출력하자. -->
<div id="d_msg"></div>
<table id="dg_emp" class="easyui-datagrid" data-options="url:'../emp.json'" width="950" title="사원관리">
<thead>
	<tr>
		<th data-options="field:'EMPNO', align:'center', width:100">사원번호</th>
		<th data-options="field:'DEPTNO', align:'center', width:200, 
						  formatter: function(value,row,index){
									  return row.DEPTNO;
						  },
						  editor:{
						  	 type:'combobox'
						  	,options:{
						  		 valueField:'DEPTNO'
						  		,textField:'DNAME'
						  		,method:'get'
						  		,url:'../dept.json'
						  	}
						  }
		">부서번호</th>
	</tr>
</thead>
</table>
</body>
</html>
```

## empManagerCtype - html+Js

![API&#xB97C; &#xD65C;&#xC6A9;&#xD574; &#xD574;&#xB2F9; row&#xB97C; &#xC120;&#xD0DD;&#xD588;&#xC744; &#xB54C; &#xBD80;&#xC11C;&#xBC88;&#xD638; cell&#xC5D0; combobox&#xB97C; &#xAD6C;&#xD604;&#xD574;&#xBCF4;&#xC790;](../../.gitbook/assets/6%20%2815%29.png)

```markup
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>사원관리(empManagerCType.html)</title>
<!-- 공통코드 추가 시작 
나중에는 이것들을 include를 사용해서 한 줄 추가로 하면 좋을거 같아요.-->
    <link rel="stylesheet" type="text/css" href="https://www.jeasyui.com/easyui/themes/default/easyui.css">
    <link rel="stylesheet" type="text/css" href="https://www.jeasyui.com/easyui/themes/icon.css">
    <link rel="stylesheet" type="text/css" href="https://www.jeasyui.com/easyui/themes/color.css">
    <link rel="stylesheet" type="text/css" href="https://www.jeasyui.com/easyui/demo/demo.css">
    <script type="text/javascript" src="https://www.jeasyui.com/easyui/jquery.min.js"></script>
    <script type="text/javascript" src="https://www.jeasyui.com/easyui/jquery.easyui.min.js"></script>
<!-- 공통코드 추가   끝  -->
   <script type="text/javascript">
       var v_date;//사용자가 선택한 날짜 정보를 담을 변수 선언.      
         
         var editIndex = undefined;
         function endEditing(){
             if (editIndex == undefined){return true}
             if ($('#dg_emp').datagrid('validateRow', editIndex)){
                 $('#dg_emp').datagrid('endEdit', editIndex);
                 editIndex = undefined;
                 return true;
             } else {
                 return false;
             }
         }
         function onClickCell(index, field){
             if (editIndex != index){
                 if (endEditing()){
                     $('#dg_emp').datagrid('selectRow', index)
                             .datagrid('beginEdit', index);
                     var ed = $('#dg_emp').datagrid('getEditor', {index:index,field:field});
                     if (ed){
                         ($(ed.target).data('textbox') ? $(ed.target).textbox('textbox') : $(ed.target)).focus();
                     }
                     editIndex = index;
                 } else {
                     setTimeout(function(){
                         $('#dg_emp').datagrid('selectRow', editIndex);
                     },0);
                 }
             }
         }
         function onEndEdit(index, row){
             var ed = $(this).datagrid('getEditor', {
                 index: index,
                 field: 'EMPNO'
             });
            // row.DEPTNO = $(ed.target).combobox('getText');
         }                 
   </script>
</head>
<body>
<script type="text/javascript">   
   $(document).ready(function (){
      //1.datagrid에 대한 초기화
	   $("#dg_emp").datagrid({
            url: "../../getEmpList.jsp"
           ,columns:[[
        	   	{field:'empno'   , title:'사원번호', width:100, editor:'text'}
        	   ,{field:'ename'	 , title:'사원이름', width:100, editor:'text'}
        	   ,{field:'job'	   , title:'직급'   , width:100, editor:'text'}
        	   ,{field:'mgr'     , title:'그룹번호', width:100, editor:'text'}
        	   ,{field:'hiredate', title:'입사일자', width:100, editor:'text'}
        	   ,{field:'sal'     , title:'급여'   , width:100, editor:'text'}
        	   ,{field:'comm'    , title:'인센티브', width:100, editor:'text'}
        	   ,{field:'deptno'  , title:'부서번호', width:100,
        	   	  formatter: function(value,row){
						  return row.DEPTNO||value;
			 	  },
			  	  editor:{
			  		 type:'combobox'
			  		 ,options:{
			  				 valueField:'DEPTNO'//실제 서버에 넘어가는 필드
			  				,textField:'DNAME'
			  				,data:'DEPTNO'
			  				,url:'../dept.json'
			  	 	 		}
			   		 }
				 }
        	   ,{field:'status',width:60,align:'center',editor:{type:'checkbox',options:{on:'P',off:''}}}
           ]]
	   	     ,onClickCell: onClickCell
       	   ,onEndEdit: onEndEdit
        });
   });
</script>
<!-- 이 페이지에 대한 에러 메시지나 힌트문을 출력하자. -->
<div id="d_msg"></div>
<table id="dg_emp" class="easyui-datagrid" width="950" title="사원관리">

</table>
</body>
</html>

```

* API를 활용해 해당 row를 선택했을 때 부서번호 cell에 combobox를 구현해보자
* 함수는 &lt;head&gt;영역으로 빼 구현한다. -&gt; 재사용성, 독립성

