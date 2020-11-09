# empManagerHtype - VO+myBatis로 우편번호 불러오기 + row선택해제

## empManagerHtype

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

    </style>
<!-- 공통코드 추가   끝  -->
   <script type="text/javascript">
       var g_cnt=0;
       var g_empno=0;
       var zdo="";
       var dong="";
       var s_index=null;

       function zipCodeSearch(){
    	   $("#u_zipCode").combobox({
    			  valueField:'ZDO'
    			  ,textField:'ZDO'
    			  ,url:'../../getZdoList.jsp' 
    			  ,onSelect:function(rec){
    				  alert('당신이 선택한 검색조건은 '+rec.ZDO);
    				  zdo=rec.ZDO; 
    			  }
    		   });
    	   $("#dg_zipCode").datagrid({
                    singleSelect:true
                   ,columns:[[
               	   	{field:'zdo'      ,  title:'시/군/구' , width:100 , align:'center'}
               	   ,{field:'zipcode'  , title:'우편번호', width:100, align:'center'}
               	   ,{field:'address'  , title:'주소', width:200, align:'center'}
                  ]]
               });   
    	   $("#dlg_zipCode").dialog('open');//처리주체 : 브라우저   	   
       }
       
       function zipCodeSearch2(){
    	  var dong = $("#sb_dong").val()
    	  alert(zdo+", "+dong);
    	  if(dong == null||dong.length<1){
    		  alert("동을 입력하세요.");
    		  return
    	  }else{
    		  $.ajax({
				   url:'../../getZipCodeList.jsp'
				  ,datatype:'json'
				  ,method:'post'
				  ,data: {"ZDO":zdo, "DONG":dong}
    		      ,success:function(data){
    		    	  var result = JSON.stringify(data);
    		    	  alert(result);
					  var arr = JSON.parse(result);
					  alert(arr);
    	  			$("#dg_zipCode").datagrid("loadData",arr);
    		      }		
    	  	});
    	  }
       }
        function zipCodeAction(){
    	   
        }       
   </script>
</head>
<body>
<script type="text/javascript">   
   $(document).ready(function (){		   
      //1.datagrid에 대한 초기화
	   $("#dg_emp").datagrid({
	   	  onClickRow: function(index,row){
	   		  if(s_index==index){
	   			  s_index=null;
	   			  $("#dg_emp").datagrid('unselectRow',index);
	   		  }
	   		  s_index=index;
	   		  g_cnt = 1;
	   		  g_empno = row.EMPNO;	   		  
	       }
       });
});

</script>
<!---------------------------------------------------------- [[우편번호 dialog 시작]] -------------------------------------------------------------------------->
<div id="dlg_zipCode" class="easyui-dialog" style="width:500px; padding:30px 30px;" data-options="closed:true, title:'우편번호 조회', footer:'#d_zipcode'">
	<div style="margin-bottom:30px;">
		<input class="easyui-combobox" id="u_zipCode" name="deptno" style="width:100px" 
		       data-options="valueField: 'ZDO'
        					,textField: 'ZDO'
        					,prompt:'Enter a 시/군/구'">
		<input id="sb_dong" name="sb_dong" class="easyui-searchbox" style="width:150px" data-options="searcher:zipCodeSearch2,prompt:'검색할 동 입력'"></input>
	</div>
	
	<table id="dg_zipCode" class="easyui-datagrid" width="440" title="우편번호 조회" data-options="onDblClickRow:function(index,row){var zipCode=row.zipCode}">
	</table>
	
	<div>
		<!-- 저장과 닫기버튼 추가 -->
    	<!-- a태그는 문단이동(스크롤바 없이 특정문단으로 이동), link, JS함수호출 가능 -->
		<div id="d_zipcode" style="margin-top:10px; margin-bottom:5px;" align="right">   
      		<a href="javascript:zipCodeAction()" class="easyui-linkbutton" iconCls="icon-save" plain="true">확인</a>
      		<a href="javascript:$('#dlg_zipCode').dialog('close')" class="easyui-linkbutton" iconCls="icon-cancel" plain="true">닫기</a>
      	</div>
	</div>
</div>
<!----------------------------------------------------- [[우편번호 dialog 끝]] -------------------------------------------------------------------------->

</body>
</html>
```

## getZipCodeList.jsp

```java
<%@ page language="java" contentType="application/json; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ page import="java.sql.*, java.util.*" %>
<%@ page import="com.google.gson.Gson" %>
<%@ page import="web.mvc.*" %>

<%
	String zdo = request.getParameter("ZDO");
	String dong = request.getParameter("DONG");
	
	Map<String, Object> pmap = new HashMap<>();
	pmap.put("ZDO",zdo);//7566 or SMITH or 3000
	pmap.put("DONG",dong);//7566 or SMITH or 3000
	
	//쿼리문을 갱신하기 위해서는 원본이 바뀌지않는 String대신 StringBuilder를 사용한다.
	StringBuilder sql = new StringBuilder();
	ZipCodeDao zcDao = new ZipCodeDao();	
	List<ZipCodeVO> zipList = zcDao.getZipCodeList(pmap);
	Gson g = new Gson();//google제공 json API
	String temp = g.toJson(zipList);
	out.print(temp);
%>
```

## ZipCodeDao.java

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

public class ZipCodeDao {//조회하기
	Logger logger = Logger.getLogger(ZipCodeDao.class);
	String resource = "com/util/Configuration.xml";
	SqlSessionFactory sqlMapper = null;
	
	/*********************************************************************************************
	 * 제목 : 동 정보를 입력받아서 우편번호를 조회한다. (기존 java코드를 myBatis로 변경)
	 * @param dong : 사용자가 입력한 동 이름을 받는다.
	 * @return List<ZipCodeVO> - n건 조회 -> 사용자가 선택(onDblClickRow, onSelect) -> 등록 화면에 우편번호 넣기
	 * @author 한영진 2020.11.06 수정 완료
	 ********************************************************************************************/
	public List<ZipCodeVO> getZipCodeList(Map<String, Object> pmap) {//dong이 파라미터로 있어야 조회할 수 있다.
		SqlSession session = null;		
		List<ZipCodeVO> zipList = null;//VO를 사용하자
		try {
			//물리적으로 떨어져 있는 소스에서 필요한 정보를 읽어와야 함. Reader<-->Writer
			//문서를 읽는데 필요한 클래스
			Reader reader = Resources.getResourceAsReader(resource);
			sqlMapper = new SqlSessionFactoryBuilder().build(reader,"development");
			logger.info("sqlMapper"+sqlMapper);
			session = sqlMapper.openSession();
			zipList = session.selectList("getZipCodeList",pmap);
			logger.info("zipCodeList :"+zipList);		
			session.close();
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			session.close();
		} return zipList;
	}//end of getZipCodList
}//end of class
```

## zipCode.xml

```markup
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="oracle.mybatis.ZipCodeMapper">

<select id="getZipCodeList" parameterType="java.util.Map" resultType="web.mvc.ZipCodeVO">
	SELECT zdo, zipcode, address
	  FROM zipcode_t	 
	<where>
		<if test='ZDO !=null and ZDO.length>0'><!-- 문자, 문자를 포함하니? -->
			and zdo LIKE '%'||#{ZDO}||'%'
		</if>
		<if test='DONG !=null and DONG.length>0'><!-- 문자, 문자를 포함하니? -->
			and dong LIKE '%'||#{DONG}||'%'
		</if>
	</where>
 </select>
</mapper>
```

