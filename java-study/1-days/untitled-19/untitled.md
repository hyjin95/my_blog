# empManagerHtype - Ajax를 이용한 수정dialog

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
   </script>
</head>
<body>
<script type="text/javascript">   
   $(document).ready(function (){	  
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

<!-- 업무관련 버튼 추가 시작 -->
	<tr>
		<td>
			<a href="javascript:void(0)" class="easyui-linkbutton" iconCls="icon-edit"   plain="true" onclick="empUpdate()">수정</a>
		</td>
	</tr>
</table>
</div>

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
 
<select id="getEmpList3" parameterType="map" resultType="map">
	SELECT 0 CK, empno, ename, job, TO_CHAR(hiredate, 'YYYY-MM-DD') hiredate, mgr, sal, NVL(comm,0) as "COMM", deptno
	  FROM emp	 
	 WHERE empno = #{g_empno}
 </select>
</mapper>
```

