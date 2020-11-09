# empManagerDtype - JSP, MyBatis

## empManagerDtype - 화면

```markup
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
          border: 5px solid orange;
          width: 700px;
          height: 120px;
       }
    </style>
<!-- 공통코드 추가   끝  -->
   <script type="text/javascript">
       var v_date;//사용자가 선택한 날짜 정보를 담을 변수 선언.  
       var test;
         
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
		 
         /* param1 : 사용자가 searchbox에 입력한 값
         *  param2 : sreachbox에 등록한 name값, JSP나 서블릿에서 사용자가 입력한 값을 요청할때 사용한다.
         *  -> request.getParameter("sb_keyword")
         */
         function empSearch(value,name){
        	 name = value;
        	 value = test;
             alert(value+":"+name)
             //var user_input = $("#sb_keyword").val();//val또는 getValue
             //화면에 갱신처리 - 페이지를 이동해야 하나?
             //datagrid를 활용하면 현재 보고 있는 페이지에서 처리할 수 있다. --JSP, Servlet을 경유 : include
             $("#dg_emp").datagrid({
            	/* 전체조회와 조건조회를 하나의 메서드로 할 것인지 분리할 것인지 결정해야한다. 
            	*  --기준 : 전체조회를 출력하는 페이지와 조건조회 출력 페이지가 서로다른 html이라면 분리한다.--유지보수를 생각하자
            	*  분리하지않으면 if문으로 나눠야하고, 코드가 지저분해질 수 있다.
            	*  --기준1 : 한페이지에서 처리하려면 새로고침이 일어나야 한다.
            	*  select from 기본꼴 or select from where 조건꼴
            	*  조건검색 : 듣기 -> 처리
            	*  사용자가 선택하는 조건에 따라 조건검색이 달라져야한다 = 동적쿼리 = if문 -> 값이 아닌 컬럼명이 바뀌어야함
            	*  배달사고에 유의하자 - 500번 중에 하나. -> 단위테스트하기
            	*  파라미터를 가져오고 리턴값을 내려보낸다.
            	*/ 
            	url:'../../getEmpList2.jsp?cols='+value+"&keyword="+name//name을 쿼리스트링으로 넘긴다.
            	,onLoadSuccess:function(temp){
   			 		alert("onload....."+temp);
   			 		//alert("onload....."+JSON.stringify(temp));
   			 	}		
             });
         }
         
       //너 조회할거야?
         function empList(){
            $("#d_msg").append("empList호출 성공<br>");
            $("#dg_emp").datagrid({
               url: "../emp.json"/* 서버의 이전이나 소스의 재사용성을 고려하여 상대경로로 작성할것. */   
              
            });
      }                  
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
            url: "../../getEmpList2.jsp"
            ,toolbar:'#tb_emp'
            ,columns:[[
        	   	{field:'EMPNO'   , title:'사원번호', width:100, editor:'text'}
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
	   	   ,onClickCell: onClickCell
       	   ,onEndEdit: onEndEdit
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
			<a href="javascript:void(0)" class="easyui-linkbutton" iconCls="icon-add" plain="true" onclick="addEmp()">입력</a>
			<a href="javascript:void(0)" class="easyui-linkbutton" iconCls="icon-edit" plain="true" onclick="editEmp()">수정</a>
			<a href="javascript:void(0)" class="easyui-linkbutton" iconCls="icon-remove" plain="true" onclick="removeEmp()">삭제</a>
			<!-- 사원목록 툴바 추가 끝 -->
		</td>
	</tr>
</table>
</div>
<!------------------------------------------------------------ 툴바 추가 끝 ---------------------------------------------------------------------------->

<table id="dg_emp" class="easyui-datagrid" width="950" title="사원관리">
</table>
</body>
</html>
```

## getEmpList2.jsp

```java
<%@ page language="java" contentType="application/json; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ page import="java.sql.*, java.util.*" %>
<%@ page import="com.google.gson.Gson" %>
<%@ page import="web.mvc.*" %>

<%
	String cols = request.getParameter("cols");
	String keyword = request.getParameter("keyword");
	
	Map<String, Object> pmap = new HashMap<>();
	if(cols!=null){
		if("empno".equals(cols)){
			pmap.put("uempno","empno");
		}
		else if("ename".equals(cols)){
			pmap.put("uename","ename");
		}
		else if("sal".equals(cols)){
			pmap.put("usal","sal");
		}
	}
	//키워드 비교
	if(keyword!=null){
		pmap.put("keyword",keyword);//7566 or SMITH or 3000
	}
	
	//쿼리문을 갱신하기 위해서는 원본이 바뀌지않는 String대신 StringBuilder를 사용한다.
	StringBuilder sql = new StringBuilder();
	EmpDao dDao = new EmpDao();	
	List<Map<String,Object>> empList = dDao.getEmpList(pmap);
	Gson g = new Gson();//google제공 json API
	String temp = g.toJson(empList);
	//out.print(deptList); JSON이 아닌 String덩어리로 나와버림
	out.print(temp);
%>
```

## EmoDao.java

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
}
```

## Configuration.xml

```markup
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration
 PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
 "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
	<environments default="development">
		<environment id="development">
			<transactionManager type="JDBC" />
			<dataSource type="POOLED">
				<property name="driver" value="oracle.jdbc.driver.OracleDriver" />
				<property name="url" value="jdbc:oracle:thin:@192.168.0.187:1521:orcl11" />
				<property name="username" value="scott" />
				<property name="password" value="tiger" />
			</dataSource>
		</environment>
	</environments>
	<mappers>
		<mapper resource="oracle/mybatis/emp.xml"/>
	</mappers>
</configuration>
```

## emp.xml

```markup
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="oracle.mybatis.EmpMapper">

<select id="getEmpList" parameterType="map" resultType="map">
	SELECT empno, ename, job, TO_CHAR(hiredate, 'YYYY-MM-DD') hiredate, mgr, sal, comm, deptno
	  FROM emp	 
	<where>
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

</mapper>
```



