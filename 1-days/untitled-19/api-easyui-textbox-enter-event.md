# API활용 - easyui-textbox : Enter Event구현

```markup
<body>
<script type="text/javascript">   
   $(document).ready(function (){	  
	  //2.콤보박스 초기화 및 설정
	  $("#cb_search").combobox({
		  data:[
			   {cols:'empno', label:'사원번호'}
			  ,{cols:'ename', label:'사원이름'}
			  ,{cols:'sal'  , label:'급여'}
		  ]
	  	 ,onSelect:function(rec){//콥보박스에서 선택했을떄 @param:object로서 row의 주소번지를 갖는다. row.empno, row.ename
	  		alert('당신이 선택한 검색조건은 '+rec.cols);
			//var url = '../../getEmpList2.jsp?cols='+rec.cols;
			test=rec.cols;
	  	 }
	  });
	   
      //1.datagrid에 대한 초기화
	   $("#dg_emp").datagrid({
            toolbar:'#tb_emp'
            ,singleSelect:true
            ,columns:[[
        	   	{field:'CK'      , checkbox:true , width:50 , align:'center'}
        	   ,{field:'EMPNO'   , title:'사원번호', width:100, editor:'text'}
        	   ,{field:'ENAME'	 , title:'사원이름', width:150, editor:'text'}
        	   ,{field:'JOB'	 , title:'직급'   , width:160, editor:'text'}
        	   ,{field:'MGR'     , title:'그룹번호', width:100, editor:'text', hidden:'true'}
        	   ,{field:'SAL'     , title:'급여'   , width:100, editor:'text'}
        	   ,{field:'HIREDATE', title:'입사일자', width:100, editor:'text', hidden:'true'}
        	   ,{field:'COMM'    , title:'인센티브', width:100, editor:'text'}
        	   ,{field:'DEPTNO'  , title:'부서번호', width:100,
     	   	  editor:{
			  		 type:'combobox'
			  		 ,options:{
			  				 valueField:'deptno'//실제 서버에 넘어가는 필드
			  				,textField:'dname'
			  				,data:'deptno'
			  				//,method:'post' easyui를 사용해 기본이 post
			  				,url:'../dept.json'
			  	 	 		}
			   		 }
				 }
        	   ,{field:'action',title:'Action',width:140,align:'center',
                   formatter:function(value,row,index){
                       if (row.editing){
                           var s = '<a href="javascript:void(0)" onclick="saverow(this)">Save</a> ';
                           var c = '<a href="javascript:void(0)" onclick="cancelrow(this)">Cancel</a>';
                           return s+c;
                       } else {
                           var e = '<a href="javascript:void(0)" onclick="editrow(this)">Edit</a> ';
                           var d = '<a href="javascript:void(0)" onclick="deleterow(this)">Delete</a>';
                           return e+d;
                       }
                   }
               }
           ]]
     });
	   $('#tb_keyword').textbox('textbox').bind('keydown', function(e){
		   	if (e.keyCode == 13){	// when press ENTER key, accept the inputed value.
		   		empSearch2();
		   	}
		   });
   });
</script>
<!-- 이 페이지에 대한 에러 메시지나 힌트문을 출력하자. -->
<div id="d_msg"></div>
<!------------------------------------------------------ 툴바 추가 시작 ---------------------------------------------------------------->
<div id="tb_emp">
<table border="0" width="100%">
	<tr>
		<td>
			<table>
				<tr>
					<td width="300px">
						<label width="100px">사원번호 : </label>
						<input id="nb_empno" class="easyui-numberbox" value="0">
					</td>
					<td width="300px">
						<label width="100px">연봉 : </label>
						<input id="nb_sal" 	class="easyui-numberbox" value="0"	 data-options="groupSeparator:','">
					</td>
					<td width="300px">					
						<input id="dd_hiredate" label="입사일자 : " class="easyui-datebox"   value="today" required="required" style="width:200px">
					</td>
				</tr>
					<!-- combobox추가 (위치선택 -> 공간확보 -> 코드작성,추가) 이름|job|부서번호 -->
				<tr>
					<!--------------------추가 ----------------------->
					<td width="1000px" colspan="3">
						<label width="100px">검색분류 : </label>
						<input id="cb_search" class="easyui-combobox" data-options="
        					valueField: 'cols',
        					textField: 'label'
        					">
						<input id="sb_keyword" name="sb_keyword" class="easyui-searchbox" style="width:150px" 
							   data-options="searcher:empSearch,prompt:'검색할 값 입력'"></input>
						<input id="tb_keyword" name="tb_keyword" class="easyui-textbox" style="width:150px"
							   data-options="prompt:'검색할 값 입력'"></input>
        					
					</td>					
					<!-------------------- 끝 ----------------------->
				</tr>
			</table>			
		</td>		
	</tr>
<!-- 조건 검색 화면 끝 -->

<!-- 업무관련 버튼 추가 시작 -->
	<tr>
		<td>
			<!-- 사원목록 툴바 추가 -->
			<a href="javascript:void(0)" class="easyui-linkbutton" iconCls="icon-search" plain="true" onclick="getEmpList()">조회</a>
			<a href="javascript:void(0)" class="easyui-linkbutton" iconCls="icon-add"    onclick="insert()">추가</a>
			<a href="javascript:void(0)" class="easyui-linkbutton" iconCls="icon-add" plain="true" onclick="addEmp()">입력</a>
			<a href="javascript:void(0)" class="easyui-linkbutton" iconCls="icon-edit"   plain="true" onclick="empUpdate()">수정</a>
			<a href="javascript:void(0)" class="easyui-linkbutton" iconCls="icon-remove" plain="true" onclick="removeEmp()">삭제</a>
			<!-- 사원목록 툴바 추가 끝 -->
		</td>
	</tr>
</table>
</div>
<!------------------------------------------------------------[[ 툴바 추가 끝 ]]---------------------------------------------------------------------------->
<table id="dg_emp" class="easyui-datagrid" width="950" title="사원관리" data-options="onDblClickRow:function(index,row){var ename=row.ENAME}"><!-- index=몇번째row row=어떤 값 -->
</table>

<!--------------------------------------------------------- [[사원카드 등록하기 시작]] -------------------------------------------------------------------------->
<div id="dlg_empINS" class="easyui-dialog" data-options="closed:true, title:'사원정보 등록'">
	<form id="f_empINS">
		
	</form>
	<div>
	
	</div>
</div>
<!---------------------------------------------------------- [[사원카드 등록하기 끝]] -------------------------------------------------------------------------->

<!--------------------------------------------------------- [[사원카드 수정하기 시작]] -------------------------------------------------------------------------->
<div id="dlg_empUPD" class="easyui-dialog" style="width:500px; padding:30px 30px;" data-options="closed:true, title:'사원정보 수정', footer:'#d_upd'">
	<form id="f_empUPD">
		 <div style="margin-bottom:10px">
         	<input class="easyui-numberbox" id="u_empno" name="empno" label="사원번호" data-options="prompt:'Enter a EmpNO'" style="width:150px">
         </div>
         <div style="margin-bottom:10px">
         	<input class="easyui-textbox" id="u_ename" name="ename" label="사원명" data-options="prompt:'Enter a ENAME'" style="width:250px">
         </div>
         <div style="margin-bottom:10px">
         	<input class="easyui-textbox" id="u_job" name="job" label="JOB" data-options="prompt:'Enter a JOB'" style="width:250px">
         </div>
         <div style="margin-bottom:10px">
         	<input class="easyui-textbox" id="u_hiredate" name="hiredate" label="입사일자" data-options="prompt:'Enter a 입사일자'" style="width:250px">
         </div>
         <div style="margin-bottom:10px">
         	<input class="easyui-numberbox" id="u_sal" name="sal" label="급여" data-options="prompt:'Enter a 급여'" style="width:250px">
         </div>         
         <div style="margin-bottom:10px">
         	<input class="easyui-numberbox" id="u_comm" name="comm" label="인센티브"  data-options="prompt:'Enter a 인센티브'" style="width:250px">
         </div>         
         <div style="margin-bottom:10px">
         	<input class="easyui-combobox" id="u_deptno" name="deptno" label="부서번호" style="width:250px"
          		   data-options="prompt:'Enter a 부서번호'
                      			,valueField: 'DEPTNO'
                        		,textField: 'DNAME'
                         		,url: '../../getDeptList.jsp'
                         		,onSelect: function(rec){
                   				 }" 
         >
         </div>         
          <div style="margin-bottom:10px">
         	<input class="easyui-textbox" id="zipcode" name="zipcode" label="우편번호"  data-options="prompt:'Enter a ZIPCODE'" style="width:160px">
         	<a id="btn_zipcode" href="#" class="easyui-linkbutton">우편번호찾기</a>
         </div>

         <div style="margin-bottom:10px">
         	<input class="easyui-textbox" id="mem_addr" name="mem_addr" label="주소"  data-options="prompt:'Enter a ADDRESS'" style="width:420px">
         </div> 
	</form>
	<div>
		<!-- 저장과 닫기버튼 추가 -->
    	<!-- a태그는 문단이동(스크롤바 없이 특정문단으로 이동), link, JS함수호출 가능 -->
		<div id="d_upd" style="margin-top:10px; margin-bottom:5px;" align="right">   
      		<a href="javascript:updAction()" class="easyui-linkbutton" iconCls="icon-save" plain="true">저장</a>
      		<a href="javascript:$('#dlg_empUPD').dialog('close')" class="easyui-linkbutton" iconCls="icon-cancel" plain="true">닫기</a>
      	</div>
	</div>
</div>

<!---------------------------------------------------------- [[사원카드 수정하기 끝]] -------------------------------------------------------------------------->

</body>
```

