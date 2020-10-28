# empManager - text-date-number Box, emp.json

## empManager.html

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
<style type="text/css">
	#d_msg {
		border: 5px solid pink;
		width: 700px;
		height: 120px;
	}
	#dlg_add, #dlg_edit, #dlg_remove {
		width: 250px;
		height: 220px;
	}
</style>

<script type="text/javascript">
	var v_date;//사용자가 선택한 날짜 정보를 담을 변수 선언
	//조회
	function getEmpList(){
		$("#d_msg").append("empList호출 성공<br>");
		$("#dg_emp").datagrid({				
			url:'../emp.json'			
		});
	}
	//입력
	function addEmp(){
		$("#d_msg").append("addEmp호출 성공<br>");
		$("#dlg_add").dialog({
			closed: false
			,title: '입력'
			,modal: true//배경, 뒷 화면 비활성화
		});
	}
	//수정
	function editEmp(){
		$("#d_msg").append("editEmp호출 성공<br>");
		$("#dlg_edit").dialog({
			closed: false
			,title: '수정'
			,modal: false
		});
	}
	//삭제
	function removeEmp(){
		$("#d_msg").append("removeEmp호출 성공<br>");
		$("#dlg_remove").dialog({
			closed: false
			,title: '삭제'
			,modal: false
		});
		//delete from dept where deptno=?
	}
</script>
</head>
<body>
<!-- 이 페이지에 대한 에러 메세지나 힌트문을 출력하는 영역 -->
<div id="d_msg"></div>
<!-- 이 페이지에 대한 에러 메세지나 힌트문을 출력하는 영역 -->

<!-- 돔트리가 완료되었을떄
  	  태그가 먼저 만들어져야 해당 태그에 추가 태그를 작성 할 수 있다.ready함수를 호출해 기존 태그들을 먼저 스캔하도록 유도한다. 
  	  그 다음에 해당 컴포넌트에 접근한다. 여러 화면이 공존할 수도 있으므로 식별 아이디를 반드시 부여하자. 
  	 JQuery : $("#아이디"), JS : document.getElementById("아이디") 
  	 JQuery를 사용해 오타 방지를 위해 조금이라도 코드를 줄인다.
  	 $("#아이디").datagrid({
  	 	속성이름: '값'
  	   ,속성이름: '값'
  	   (alert())//불법 - 함수영역에서만 사용할 수 있다.
  	   ,onSelect: function(index, row){
  	 		alert();//합법
  	 	}
  	   ,onloadSuccess: function(){
  	   
  	   }, ....
  	 }); -->
<script type="text/javascript">
	//아래 코드는 datebox에 대한 돔 구성이 완료되기 전에 디폴트 포맷정보를 수정해야하는 경우 이므로 ready이전에 미리 처리한다.
	//날짜 선택시 한국 스타일로 변경 YYYY-MM-DD
	$.fn.datebox.defaults.formatter = function(date){
		var y = date.getFullYear();
		var m = date.getMonth()+1;//달은 0월이 아닌 1월 부터라서 +1한다.
		var d = date.getDate();
		return y+"-"+(m<10? "0"+m:m)+"-"+(d<10? "0"+d:d);//삼항 연산자 응요해보기 m이랑 d가 10이하, 한자릿 수면 0을 붙이고 아니면 그대로 출력
	}
	$.fn.datebox.defaults.parser = function(s){
		var t = Date.parse(s);
		if (!isNaN(t)){
			return new Date(t);
		} else {
			return new Date();
		}
	}
	$(document).ready(function(){
		//화면 컴포넌트 - 1
		$("#dg_emp").datagrid({
			title:'사원관리 시스템'
			,fitColumns:true
			,singleSelect:true						
			,toolbar:'#tb_emp'
			,columns:[[
				 {field:'ENAME', 	title:'사원이름', width:100, align:'center'}
				,{field:'EMPNO',	title:'사원번호', width:100, align:'center'}
				,{field:'HIREDATE',	title:'입사일자', width:200, align:'center'}
				,{field:'JOB', 		title:'직급',    width:100, align:'center'}
				,{field:'DEPTNO',	title:'부서번호', width:100, align:'center'}
				,{field:'SAL', 		title:'연봉',    width:100, align:'center'}
			]]
		});
	
		//화면 컴포넌트 - 2 - numberbox
		$('#nb_empno').numberbox({
			icons: [{
				iconCls:'icon-search',
				handler: function(e){
					alert("call");//단위테스트
				}////end of handler
			}]
		});///////////end of numberbox
		
		//화면 컴포넌트 - 3 - numberbox
		$('#nb_sal').numberbox({
			min:0
		   ,precision:2
		   ,icons: [{
				iconCls:'icon-search',
				handler: function(e){
					alert("급여조회 호출 성공");//단위테스트
				}////end of handler
			}]
		});///////////end of numberbox
		
		//화면 컴포넌트 - 4 - 날짜 선택시 한국 스타일로 변경 YYYY-MM-DD

	});
</script>
<!-- 화면그리기 시작 -->
<!-- typeB권장 : 화면은 직관적이여야한다. 시각적으로 보여지는것이 좋다 = 태그로 작성하기
	 UI/UX솔루션이 변경될떄 유연하게 대처할 수 있도록 코드작성하기 
	 이벤트와 제어, 다른 API와의 연계되는 부분 : JS-->
<table id="dg_emp" width="950px"></table>

<!-- 테이블태그를 활용해 조건 검색하는 화면을 추가하고 그 아래에 버튼을 배치하시오 -->
<div id="tb_emp">
<table border="0" width="100%">
<!-- 조건 검색 화면 시작 -->
	<tr>
		<td>
			<input id="nb_empno"	label="사원번호 :" class="easyui-numberbox" value="0">
			<input id="nb_sal" 		label="연봉 :" 	class="easyui-numberbox" value="0"	   data-options="groupSeparator:','">
			<input id="dd_hiredate" label="입사일자 :" class="easyui-datebox"	 value="today" required="required" style="width:200px">
		</td>		
	</tr>
<!-- 조건 검색 화면 끝 -->

<!-- 업무관련 버튼 추가 시작 -->
	<tr>
		<td>
			<!-- 사원목록 툴바 추가 -->
			<a href="javascript:void(0)" class="easyui-linkbutton" iconCls="icon-search" plain="true" onclick="getEmpList()">조회</a>
			<a href="javascript:void(0)" class="easyui-linkbutton" iconCls="icon-add" plain="true" onclick="addEmp()">입력</a>
			<a href="javascript:void(0)" class="easyui-linkbutton" iconCls="icon-edit" plain="true" onclick="editEmp()">수정</a>
			<a href="javascript:void(0)" class="easyui-linkbutton" iconCls="icon-remove" plain="true" onclick="removeEmp()">삭제</a>
			<!-- 사원목록 툴바 추가 끝 -->
		</td>
	</tr>
</table>
</div>
<!-- 업무관련 버튼 추가 끝 -->


<!-- dialog 모달 화면 추가 (등록화면|수정|삭제) -->
<div id="dlg_add" class="easyui-dialog" data-options="closed:true">
	<input id="tb_ename"
       	   class="easyui-textbox" label="사원이름 : " 
       	   style="width:150px; padding:5px;">
    <br>
	<input id="tb_empno"
       	   class="easyui-textbox" label="사원번호 : " 
       	   style="width:150px; padding:5px;">
    <br>
	<input id="tb_hiredate"
       	   class="easyui-textbox" label="입사일자 : " 
       	   style="width:150px;">
    <br>
	<input id="tb_job"
       	   class="easyui-textbox" label="직　　급 : " 
       	   style="width:200px; padding:5px;">
    <br>
	<input id="tb_deptno"
       	   class="easyui-textbox" label="부서번호 : " 
       	   style="width:150px; padding:5px;">
    <br>
	<input id="tb_sal"
       	   class="easyui-textbox" label="연　　봉 : " 
       	   style="width:150px; padding:5px;">
    <br>
    <button>전송</button>
</div>

<!-- dialog 모달 화면 추가 (등록|수정화면|삭제) -->

<!-- dialog 모달 화면 추가 (등록|수정|삭제화면) -->

<!-- 화면그리기 끝 -->
</body>
</html>
```

## 실행

![&#xCD08;&#xAE30; &#xD654;&#xBA74;](../../.gitbook/assets/2%20%2839%29.png)

![&#xC870;&#xD68C;&#xBC84;&#xD2BC; &#xD074;&#xB9AD;&#xC2DC;](../../.gitbook/assets/3%20%2832%29.png)

![&#xC785;&#xB825;&#xBC84;&#xD2BC; &#xD074;&#xB9AD;&#xC2DC;](../../.gitbook/assets/1%20%2851%29.png)

![datebox &#xD074;&#xB9AD;&#xC2DC;](../../.gitbook/assets/4%20%2826%29.png)

