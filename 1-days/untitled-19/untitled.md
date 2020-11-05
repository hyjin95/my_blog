# Untitled

## empManagerHtype.jsp

```markup
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>사원관리(empManagerDType.html)</title>
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
          border: 5px solid pink;
          width: 700px;
          height: 120px;
       }
       	#dlg_empINS {
			width: 250px;
			height: 380px;
			padding: 30px 30px;
	}
    </style>
<!-- 공통코드 추가   끝  -->
   <script type="text/javascript">
       var v_date;//사용자가 선택한 날짜 정보를 담을 변수 선언.  
       var test;
       var editIndex = undefined;
       
      //사용자가 선택한 row의 갯수 
      //선택은 개발자가아닌 사용자, 업무담당자가 한다.
      //컬럼을 선택할 것인가 로우를 선택할 것인가?
      //한번 클릭으로 정할 것인가 더블 클릭으로 정할 것인가?
       var g_cnt=0;
       var g_empno=0;
       
       function getEmpList(){
    	   $("#dg_emp").datagrid({
               url: "../../getEmpList2.jsp" });
       }
       function addEmp(){
    	   insert();
    	   $("#dlg_empINS").dialog({
	   			closed: false
   				,title: '입력'
   				,modal: true//배경, 뒷 화면 비활성화
   		   });
       }     
       
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
		 
         /* param1 : 사용자가 searchbox에 입력한 값
         *  param2 : sreachbox에 등록한 name값, JSP나 서블릿에서 사용자가 입력한 값을 요청할때 사용한다.
         *  -> request.getParameter("sb_keyword")
         */
         function empSearch(){
        	 
             $("#dg_emp").datagrid({            	
            	url:'../../getEmpList2.jsp?cols='+$("#cb_search").val()+"&keyword="+$("#sb_keyword").val()//name을 쿼리스트링으로 넘긴다.
            	,onLoadSuccess:function(temp){            		
            		var result = JSON.stringify(temp);   
   			 		alert("onload....."+result);
            		var test = result.split('"rows":',2);
            		var test2 = test[1].substring(0,test[1].length-1);
   			 		alert("onload....."+test2); 
   			 		var result2 = JSON.parse(test2);
   			 		alert("onload....."+result2[0].ENAME);    			 		
   			 	}		
             });
         }
         function empSearch2(){
             $("#dg_emp").datagrid({            	
            	url:'../../getEmpList2.jsp?cols='+$("#cb_search").val()+"&keyword="+$("#tb_keyword").val()//name을 쿼리스트링으로 넘긴다.
            	,onLoadSuccess:function(temp){            		
            		var result = JSON.stringify(temp);   
            		var test = result.split('"rows":',2);
            		var test2 = test[1].substring(0,test[1].length-1);
   			 		var result2 = JSON.parse(test2);
   			 	}		
             });
         }
         function empSearch3(){
        	 var keyword;//사용자가 입력한 값 담기
        	 if($("#tb_keyword").val().length>0){
        		 keyword=$("#tb_keyword").val();
        	 }
        	 if($("#sb_keyword").val().length>0){
        		 keyword=$("#sb_keyword").val();
        	 }
        	 $("#d_msg").append(tb_keyword+", "+sb_keyword+", "+keyword+"<br>");
             $("#dg_emp").datagrid({            	
            	 url:'../../getEmpList2.jsp?cols='+$("#cb_search").val()+"&keyword="+keyword//get방식 쿼리스트링
            	 //url:'../../getEmpList2.jsp?cols='+$("#cb_search").val()+"&keyword="+keyword+"&timestamp="+new Date().getTime()//초단위로 찍어줘 서버까지 전달된다.
            	,method:'post'//
            	,onLoadSuccess:function(temp){            		
            		var result = JSON.stringify(temp);   
            		var test = result.split('"rows":',2);
            		var test2 = test[1].substring(0,test[1].length-1);
   			 		var result2 = JSON.parse(test2);
   			 	}		
             });
            //초기화
        	 keyword='';
        	 $("#tb_keyword").textbox('setValue','');
        	 $("#sb_keyword").searchbox('setValue','');
         }
         
       //너 조회할거야?
         function empList(){
            $("#d_msg").append("empList호출 성공<br>");
            $("#dg_emp").datagrid({
               url: "../emp.json"/* 서버의 이전이나 소스의 재사용성을 고려하여 상대경로로 작성할것. */   
              
            });         
       }  
       //////////////////////////////[[데이터그리드 입력|수정|삭제 구현]]//////////////////////////////////////
       	function empUpdate(){//수정
    	   //$.messeger.progress();
       	   //g_cnt : 선택한 row갯수, g_empno : 선택한 row의 empno
       	   alert("g_cnt : "+g_cnt+", "+"g_empno : "+g_empno);
    	   if(g_cnt==0||g_empno==0){    	   
    		   $.messager.alert('INFO','수정할 사원을 선택하시오.');
    		   return;//empUpdate() 탈출
    	   }else {
			   //dialog오픈 전 - 화면이 열리기 전에 data를 DB에서 가져다 놔야 한다. 
			   //처리주체 : 서버(SQL) -> 결과는 1건이다. PK로 검색할 것이므로
			   $.ajax({
				   url:'../../getEmpList3.jsp'
				  ,datatype:'json'
				  ,method:'post'
				  ,data: {"empno":g_empno}
				  ,success:function(data){
					  $("#d_msg").append(data);//json포맷 형식 출력
					  var result = JSON.stringify(data);
					  $("#d_msg").append(result);//json포맷 형식 출력
					  var arr = JSON.parse(result);
					  for(var i=0;i<arr.length;i++){
						  $("#u_empno").numberbox('setValue', arr[i].EMPNO);//UI초기화
						  $("#u_ename").textbox('setValue', arr[i].ENAME);//UI초기화
						  $("#u_job").textbox('setValue', arr[i].JOB);//UI초기화
						  $("#u_hiredate").textbox('setValue', arr[i].HIREDATE);//UI초기화
						  $("#u_sal").numberbox('setValue', arr[i].SAL);//UI초기화
						  $("#u_comm").numberbox('setValue', arr[i].COMM);//UI초기화
						  $("#u_deptno").combobox('setValue', arr[i].DEPTNO);//UI초기화
					  }
				  }
			   });			   
			   //dialog오픈 이후
    		   $("#dlg_empUPD").dialog('open');//처리주체 : 브라우저    		   
    	   }    	  
        } 
        function updAction(){
    	   
        }
       	function insert(){//입력
			var row = $('#dg_emp').datagrid('getSelected');//선택한 row의 번호를 가져온다.=tbody
			$("#d_msg").append('row : '+row+'<br>');//단위테스트용
			if (row){//row = 0~n, row가 true라면, row를 선택했다면
				var index = $('#dg_emp').datagrid('getRowIndex', row);
			} else {
				index = 0;
			}
			$('#dg_emp').datagrid('insertRow', {
				index: index,
				//넥사크로 DataSet header꾸미기와 유사
				row: {
					 CK:0
					,EMPNO:0
					,ENAME:''
					,JOB:''
					,SAL:0.0
					,COMM:0.0
					,DETPNO:''
				}
			});
			$("#d_msg").append('index : '+index+'<br>');
			$('#dg_emp').datagrid('selectRow',index);
			$('#dg_emp').datagrid('beginEdit',index);
		}
        function getRowIndex(target){
			$("#d_msg").append('target : '+target+'<br>');
        	var tr = $(target).closest('tr.datagrid-row');
        	return parseInt(tr.attr('datagrid-row-index'));
        }
       	function editrow(target){
       	    $('#dg_emp').datagrid('beginEdit', getRowIndex(target));
       	}
       	function deleterow(target){
       	    $.messager.confirm('Confirm','Are you sure?',function(r){
       	        if (r){
       	            $('#dg_emp').datagrid('deleteRow', getRowIndex(target));
					$("#d_msg").append('지정 row : '+getRowIndex(target)+'<br>');
       	        }
       	    });
       	}
       	function saverow(target){
       	    $('#dg_emp').datagrid('endEdit', getRowIndex(target));
       	}
       	function cancelrow(target){
       	    $('#dg_emp').datagrid('cancelEdit', getRowIndex(target));
       	}
       //////////////////////////////[[데이터그리드 입력|수정|삭제 구현]]//////////////////////////////////////
   </script>
</head>
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
        		   //formatter: function(value,row){
				  //	  return row.DEPTNO||value;
			 	  //},
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
	   	  ,onDblClickRow: function(index,row){
	   		  g_cnt = 1;
	   		  g_empno = row.EMPNO;
	       }
	   	   //,onClickCell: onClickCell
       	   //,onEndEdit: onEndEdit       	   
       	   //////////////////////////////////////////[[데이터그리드 이벤트핸들러]]/////////////////////////////////////////////////
      	  /*
       	   ,onEndEdit:function(index,row){
            	var ed = $(this).datagrid('getEditor', {
               	 index: index,
               	 field: 'productid'
           	 });
           	 row.productname = $(ed.target).combobox('getText');
       	  }*/
      	 ,onBeforeEdit:function(index,row){
           	 	row.editing = true;
            	$(this).datagrid('refreshRow', index);
         }
         ,onAfterEdit:function(index,row){
          		row.editing = false;
            	$(this).datagrid('refreshRow', index);
         }
         ,onCancelEdit:function(index,row){
          		row.editing = false;
            	$(this).datagrid('refreshRow', index);
        }
       	   //////////////////////////////////////////[[데이터그리드 이벤트핸들러]]/////////////////////////////////////////////////
        });
	   $('#tb_keyword').textbox('textbox').bind('keydown', function(e){
		   	if (e.keyCode == 13){	// when press ENTER key, accept the inputed value.
		   		empSearch3();
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
							   data-options="searcher:empSearch3,prompt:'검색할 값 입력'"></input>
						<input id="tb_keyword" name="tb_keyword" class="easyui-textbox" style="width:150px" onclick='empSearch3()'
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
</html>
```

## getEmpList3.jsp

```java
<%@ page language="java" contentType="application/json; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ page import="java.sql.*, java.util.*" %>
<%@ page import="com.google.gson.Gson" %>
<%@ page import="web.mvc.*" %>

<%
	int empno=0;
	String g_empno = request.getParameter("empno");
	empno = Integer.parseInt(g_empno);
	
	Map<String, Object> pmap = new HashMap<>();
	pmap.put("g_empno",empno);//7566 or SMITH or 3000
	
	//쿼리문을 갱신하기 위해서는 원본이 바뀌지않는 String대신 StringBuilder를 사용한다.
	StringBuilder sql = new StringBuilder();
	EmpDao dDao = new EmpDao();	
	List<Map<String,Object>> empList = dDao.getEmpList3(pmap);
	Gson g = new Gson();//google제공 json API
	String temp = g.toJson(empList);
	//out.print(deptList); JSON이 아닌 String덩어리로 나와버림
	out.print(temp);
%>
```

### EmpDao.java

```java
package web.mvc;

import java.io.Reader;
import java.util.List;
import java.util.Map;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.apache.log4j.Logger;

import com.util.MyBatisCommonFactory;

public class EmpDao {
	Logger logger = Logger.getLogger(EmpDao.class);
	//myBatis연동시 공통부분을 클래스로 분할해보기(반복되는 코드는 개선해본다.)
	MyBatisCommonFactory mcf = new MyBatisCommonFactory();
	//xml문서가 물리적으로 어디에 있는지-어느회사, 서버IP, port번호, 계정정보
	String resource = "com/util/Configuration.xml";
	//MyBatis에서 지원하는 클래스
	SqlSessionFactory sqlMapper = null;
	
	public List<Map<String, Object>> getEmpList(Map<String,Object> pmap){
		List<Map<String,Object>> empList = null;
		SqlSession session = null;		
		try {
			//물리적으로 떨어져 있는 소스에서 필요한 정보를 읽어와야 함. Reader<-->Writer
			//문서를 읽는데 필요한 클래스
			Reader reader = Resources.getResourceAsReader(resource);
			sqlMapper = new SqlSessionFactoryBuilder().build(reader,"development");
			logger.info("sqlMapper"+sqlMapper);
			//아래에서 생성한 session은 insert(), update(), delete(), selectOne, selectList, selectMap을 사용하기 위함이다.
			session = sqlMapper.openSession();
			empList = session.selectList("getEmpList",pmap);
			logger.info("empList:"+empList);
			session.close();
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			session.close();
		}
		return empList;
	}
	
	public List<Map<String, Object>> getEmpList3(Map<String,Object> pmap){
		List<Map<String,Object>> empList = null;
		SqlSession session = null;		
		try {
			//물리적으로 떨어져 있는 소스에서 필요한 정보를 읽어와야 함. Reader<-->Writer
			//문서를 읽는데 필요한 클래스
			Reader reader = Resources.getResourceAsReader(resource);
			sqlMapper = new SqlSessionFactoryBuilder().build(reader,"development");
			logger.info("sqlMapper"+sqlMapper);
			//아래에서 생성한 session은 insert(), update(), delete(), selectOne, selectList, selectMap을 사용하기 위함이다.
			session = sqlMapper.openSession();
			empList = session.selectList("getEmpList3",pmap);
			logger.info("empList:"+empList);
			session.close();
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			session.close();
		}
		return empList;
	}
}
```

## emp.xml

```markup
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="oracle.mybatis.EmpMapper">

<select id="getEmpList" parameterType="map" resultType="map">
	SELECT 0 CK, empno, ename, job, TO_CHAR(hiredate, 'YYYY-MM-DD') hiredate, mgr, sal, NVL(comm,0) as "COMM", deptno
	  FROM emp	 
	<where>
		<if test='uempno !=null'>
			and empno = #{keyword}
		</if>
		<if test='uempno !=null'>
			and empno = #{keyword}
		</if>
		<if test='uename !=null and uename.length>0'><!-- 문자, 문자를 포함하니? -->
			and ename LIKE '%'||#{keyword}||'%'
		</if>
		<if test='usal !=null'><!-- sal=값 존재하니? -->
			and sal = #{keyword}
		</if>
	</where>
 </select>
 
<select id="getEmpList3" parameterType="map" resultType="map">
	SELECT 0 CK, empno, ename, job, TO_CHAR(hiredate, 'YYYY-MM-DD') hiredate, mgr, sal, NVL(comm,0) as "COMM", deptno
	  FROM emp	 
	 WHERE empno = #{g_empno}
 </select>
</mapper>
```

