# nexa - xml로 dataset 전송하기

## empManager.jsp - 실행화면

![&#xC870;&#xD68C;&#xBC84;&#xD2BC; &#xD074;&#xB9AD;&#xC2DC;](../../.gitbook/assets/1%20%2854%29.png)

![&#xB125;&#xC0AC;&#xD06C;&#xB85C; &#xD568;&#xC218; &#xD638;&#xCD9C; &#xC131;&#xACF5;](../../.gitbook/assets/2%20%2842%29.png)

![&#xACB0;&#xACFC;](../../.gitbook/assets/3%20%2835%29.png)

```markup
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<script type="text/javascript">
	location.replace("http://192.168.0.187:9000/nexa2020/nexa/quickview.html?screenid=Desktop_screen&formname=Base::empManager.xfdl");
</script>
</body>
</html>
```

## Nexacro

```java
this.Edit00_onchanged = function(obj:nexacro.Edit,e:nexacro.ChangeEventInfo)
{
	
};

this.btn_search_onclick = function(obj:nexacro.Button,e:nexacro.ClickEventInfo)
{
	alert("조회 호출성공");
	//page에대한(화면)이름, URL::jsp파일이름, 입력값(JSP파일안의 입력을 받는 변수), 화면출력값,"", 콜백함수이름
	//"값"에서 "를 빼면 변수 취급해버린다. 값X
	this.transaction("empSearch", "SvcURL::llll.emp", "in_emp=ds_emp", "ds_emp=out_emp","","fn_callback");
};

this.fn_callback = function(svcID,errCD, errMSG) {
	alert("fn_callback 호출 성공 : "+svcID+", "+errCD+", "+errMSG);
};

```

## select\_emp.jsp - xml / nexa-API

```java
<%@ page import = "org.apache.commons.logging.*" %>
<%@ page import = "com.nexacro17.xapi.data.*" %>
<%@ page import = "com.nexacro17.xapi.tx.*" %>
<%@ page import = "java.util.*" %>
<%@ page import = "java.sql.*" %>
<%@ page import = "java.io.*" %>
<%@ page contentType = "text/xml; charset=UTF-8" %>

<%
// PlatformData
//넥사크로 화면에서 입력받은 값들을 톰캣서버에서 읽어올 수 있다. = getParameter
PlatformData out_pData = new PlatformData();
String sDept = (request.getParameter("sDept") == null) ? "" : request.getParameter("sDept");//sDept는 data일수도 dataSet일수도 있다.

//넥사크로에서 제공되는 에러코드번호나 메세지를 관리하는 변수가 필요하다
int    nErrorCode  = 0;
String strErrorMsg = "START";

try {    
	/******* JDBC Connection *******/	
	Connection conn = null;//물리적으로 거리가 있는 Oracle서버에 연결통로 선언
	Statement  stmt = null;//쿼리문을 담는 전령 객체 선언
	ResultSet  rs   = null;//커서조작 객체 선언
	
	//ip로 접근하므로 예외처리 필수
	try { 
		//오라클 회사 제품인것을 확인
		Class.forName("oracle.jdbc.driver.OracleDriver");//드라이버이름
		//오라클서버의 물리적 정보
		conn = DriverManager.getConnection("jdbc:oracle:thin:@192.168.0.187:1521:orcl11","scott","tiger");//바라보는 서버제품 
		//dml문을 자바에서 오라클 서버로 전달하는 클래스로 전령 생성
		stmt = conn.createStatement();
	  
		/******* SQL ************/
		String SQL;//쿼리문 담기
		SQL  = "SELECT empno    \n" +
			   "     , ename    \n" +
			   "     , job      \n" +
			   "     , mgr      \n" +
			   "     , TO_CHAR(hiredate, 'YYYY--MM-DD')hiredate \n" +//반드시 알리아스명을 줘야 찾을 수 있다.
			   "     , sal      \n" +
			   "     , comm     \n" +
			   "     , deptno   \n" +
			   "  FROM emp      \n" +
			   " WHERE 1=1      \n" ;		
				
		System.out.println(SQL);//작성한 쿼리문을 토드에서 테스트해보고싶다면 출력해서 확인한다.     
		rs = stmt.executeQuery(SQL);//오라클에게 DML문 처리 요청 모드
		DataSet ds = new DataSet("out_emp");//넥사크로에서 그린 화면 grid에 매칭될 dataset객체 생성
		//화면에 매칭(id:ds_emp)할 데이터셋 자바에서 초기화 하기 초기화 = 선언->인스턴스->값 부여
		ds.addColumn("empno"   ,DataTypes.STRING  ,(short)10 );
		ds.addColumn("ename"   ,DataTypes.STRING  ,(short)50 );
		ds.addColumn("job"     ,DataTypes.STRING  ,(short)10 );
		ds.addColumn("mgr"     ,DataTypes.STRING  ,(short)10 );
		ds.addColumn("hiredate",DataTypes.STRING  ,(short)10 );
		ds.addColumn("sal"     ,DataTypes.STRING  ,(short)10 );
		ds.addColumn("comm"    ,DataTypes.STRING  ,(short)10 );
		ds.addColumn("deptno"  ,DataTypes.STRING  ,(short)10 );
		
		//다시 조회 row수만큼 반복하면서 63번에서 추가될 공간을 먼저 확보한다.
		while(rs.next())
		{
			int row = ds.newRow();
			//65번부터 읽어온 정보를 dataset안에 초기화 해준다.
			ds.set(row ,"empno"       ,rs.getString("empno"));
			ds.set(row ,"ename"       ,rs.getString("ename"));
			ds.set(row ,"job"         ,rs.getString("job"));
			ds.set(row ,"mgr"         ,rs.getString("mgr"));
			ds.set(row ,"hiredate"    ,rs.getString("hiredate"));
			ds.set(row ,"sal"         ,rs.getString("sal"));
			ds.set(row ,"comm"        ,rs.getString("comm"));
			ds.set(row ,"deptno"      ,rs.getString("deptno"));
		}
		  
		// #1 dataset -> PlatformData
		out_pData.addDataSet(ds);

		// #2 dataset -> PlatformData
		//DataSetList dsList = out_pData.getDataSetList();
		//dsList.add(ds);

		nErrorCode  = 0;
		strErrorMsg = "SUCC";
		
	} catch (SQLException e) {
		nErrorCode = -1;
		strErrorMsg = e.getMessage();
	}    
	
	/******** JDBC Close ********/
	if ( stmt != null ) try { stmt.close(); } catch (Exception e) {nErrorCode = -1; strErrorMsg = e.getMessage();}
	if ( conn != null ) try { conn.close(); } catch (Exception e) {nErrorCode = -1; strErrorMsg = e.getMessage();}
			
	} catch (Throwable th) {
		nErrorCode = -1;
		strErrorMsg = th.getMessage();
}

VariableList varList = out_pData.getVariableList();
varList.add("ErrorCode", nErrorCode);
varList.add("ErrorMsg" , strErrorMsg);


HttpPlatformResponse pRes = new HttpPlatformResponse(response, PlatformType.CONTENT_TYPE_XML, "utf-8");
pRes.setData(out_pData);

// Send data
pRes.sendData();
%>
```

## select\_emp.java - servlet

### 선언부

```java
package nexa2020;

import java.io.IOException;
import java.io.PrintWriter;
import java.io.Reader;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.apache.log4j.Logger;

import com.google.gson.Gson;
import com.nexacro17.xapi.data.DataSet;
import com.nexacro17.xapi.data.DataTypes;
import com.nexacro17.xapi.data.PlatformData;
import com.nexacro17.xapi.data.VariableList;
import com.nexacro17.xapi.tx.HttpPlatformResponse;
import com.nexacro17.xapi.tx.PlatformException;
import com.nexacro17.xapi.tx.PlatformType;

public class select_emp extends HttpServlet {	
	Logger logger = Logger.getLogger(select_emp.class);
```

### doGet메서드

```java
	public void doGet(HttpServletRequest req, HttpServletResponse res) 
	throws ServletException, IOException {
		PlatformData out_pData = new PlatformData();
		String sDept = (req.getParameter("sDept") == null) ? "" : req.getParameter("sDept");//sDept는 data일수도 dataSet일수도 있다.

		//넥사크로에서 제공되는 에러코드번호나 메세지를 관리하는 변수가 필요하다
		int    nErrorCode  = 0;
		String strErrorMsg = "START";

		try {    
			/******* JDBC Connection *******/	
			Connection conn = null;//물리적으로 거리가 있는 Oracle서버에 연결통로 선언
			Statement  stmt = null;//쿼리문을 담는 전령 객체 선언
			ResultSet  rs   = null;//커서조작 객체 선언
			
			//ip로 접근하므로 예외처리 필수
			try { 
				//오라클 회사 제품인것을 확인
				Class.forName("oracle.jdbc.driver.OracleDriver");//드라이버이름
				//오라클서버의 물리적 정보
				conn = DriverManager.getConnection("jdbc:oracle:thin:@192.168.0.187:1521:orcl11","scott","tiger");//바라보는 서버제품 
				//dml문을 자바에서 오라클 서버로 전달하는 클래스로 전령 생성
				stmt = conn.createStatement();
			  
				/******* SQL ************/
				String SQL;//쿼리문 담기
				SQL  = "SELECT empno    \n" +
					   "     , ename    \n" +
					   "     , job      \n" +
					   "     , mgr      \n" +
					   "     , TO_CHAR(hiredate, 'YYYY--MM-DD')hiredate \n" +//반드시 알리아스명을 줘야 찾을 수 있다.
					   "     , sal      \n" +
					   "     , comm     \n" +
					   "     , deptno   \n" +
					   "  FROM emp      \n" +
					   " WHERE 1=1      \n" ;		
						
				System.out.println(SQL);//작성한 쿼리문을 토드에서 테스트해보고싶다면 출력해서 확인한다.     
				rs = stmt.executeQuery(SQL);//오라클에게 DML문 처리 요청 모드
				DataSet ds = new DataSet("out_emp");//넥사크로에서 그린 화면 grid에 매칭될 dataset객체 생성
				//화면에 매칭(id:ds_emp)할 데이터셋 자바에서 초기화 하기 초기화 = 선언->인스턴스->값 부여
				ds.addColumn("empno"   ,DataTypes.STRING  ,(short)10 );
				ds.addColumn("ename"   ,DataTypes.STRING  ,(short)50 );
				ds.addColumn("job"     ,DataTypes.STRING  ,(short)10 );
				ds.addColumn("mgr"     ,DataTypes.STRING  ,(short)10 );
				ds.addColumn("hiredate",DataTypes.STRING  ,(short)10 );
				ds.addColumn("sal"     ,DataTypes.STRING  ,(short)10 );
				ds.addColumn("comm"    ,DataTypes.STRING  ,(short)10 );
				ds.addColumn("deptno"  ,DataTypes.STRING  ,(short)10 );
				
				//다시 조회 row수만큼 반복하면서 63번에서 추가될 공간을 먼저 확보한다.
				while(rs.next())
				{
					int row = ds.newRow();
					//65번부터 읽어온 정보를 dataset안에 초기화 해준다.
					ds.set(row ,"empno"       ,rs.getString("empno"));
					ds.set(row ,"ename"       ,rs.getString("ename"));
					ds.set(row ,"job"         ,rs.getString("job"));
					ds.set(row ,"mgr"         ,rs.getString("mgr"));
					ds.set(row ,"hiredate"    ,rs.getString("hiredate"));
					ds.set(row ,"sal"         ,rs.getString("sal"));
					ds.set(row ,"comm"        ,rs.getString("comm"));
					ds.set(row ,"deptno"      ,rs.getString("deptno"));
				}
				  
				// #1 dataset -> PlatformData
				out_pData.addDataSet(ds);

				// #2 dataset -> PlatformData
				//DataSetList dsList = out_pData.getDataSetList();
				//dsList.add(ds);

				nErrorCode  = 0;
				strErrorMsg = "SUCC";
				
			} catch (SQLException e) {
				nErrorCode = -1;
				strErrorMsg = e.getMessage();
			}    
			
			/******** JDBC Close ********/
			if ( stmt != null ) try { stmt.close(); } catch (Exception e) {nErrorCode = -1; strErrorMsg = e.getMessage();}
			if ( conn != null ) try { conn.close(); } catch (Exception e) {nErrorCode = -1; strErrorMsg = e.getMessage();}
					
			} catch (Throwable th) {
				nErrorCode = -1;
				strErrorMsg = th.getMessage();
		}

		VariableList varList = out_pData.getVariableList();
		varList.add("ErrorCode", nErrorCode);
		varList.add("ErrorMsg" , strErrorMsg);

		HttpPlatformResponse pRes = new HttpPlatformResponse(res, PlatformType.CONTENT_TYPE_XML, "utf-8");
		pRes.setData(out_pData);

		// Send data
		try {
			pRes.sendData();
		} catch (PlatformException e) {
			e.printStackTrace();
		}
		logger.info("doGet호출성공");
	}
```

### doPost메서드

```java
	public void doPost(HttpServletRequest req, HttpServletResponse res) 
			throws ServletException, IOException {
		PlatformData out_pData = new PlatformData();
		String sDept = (req.getParameter("sDept") == null) ? "" : req.getParameter("sDept");//sDept는 data일수도 dataSet일수도 있다.

		//넥사크로에서 제공되는 에러코드번호나 메세지를 관리하는 변수가 필요하다
		int    nErrorCode  = 0;
		String strErrorMsg = "START";

		try {    
			/******* JDBC Connection *******/	
			Connection conn = null;//물리적으로 거리가 있는 Oracle서버에 연결통로 선언
			Statement  stmt = null;//쿼리문을 담는 전령 객체 선언
			ResultSet  rs   = null;//커서조작 객체 선언
			
			//ip로 접근하므로 예외처리 필수
			try { 
				//오라클 회사 제품인것을 확인
				Class.forName("oracle.jdbc.driver.OracleDriver");//드라이버이름
				//오라클서버의 물리적 정보
				conn = DriverManager.getConnection("jdbc:oracle:thin:@192.168.0.187:1521:orcl11","scott","tiger");//바라보는 서버제품 
				//dml문을 자바에서 오라클 서버로 전달하는 클래스로 전령 생성
				stmt = conn.createStatement();
			  
				/******* SQL ************/
				String SQL;//쿼리문 담기
				SQL  = "SELECT empno    \n" +
					   "     , ename    \n" +
					   "     , job      \n" +
					   "     , mgr      \n" +
					   "     , TO_CHAR(hiredate, 'YYYY--MM-DD')hiredate \n" +//반드시 알리아스명을 줘야 찾을 수 있다.
					   "     , sal      \n" +
					   "     , comm     \n" +
					   "     , deptno   \n" +
					   "  FROM emp      \n" +
					   " WHERE 1=1      \n" ;		
						
				System.out.println(SQL);//작성한 쿼리문을 토드에서 테스트해보고싶다면 출력해서 확인한다.     
				rs = stmt.executeQuery(SQL);//오라클에게 DML문 처리 요청 모드
				DataSet ds = new DataSet("out_emp");//넥사크로에서 그린 화면 grid에 매칭될 dataset객체 생성
				//화면에 매칭(id:ds_emp)할 데이터셋 자바에서 초기화 하기 초기화 = 선언->인스턴스->값 부여
				ds.addColumn("empno"   ,DataTypes.STRING  ,(short)10 );
				ds.addColumn("ename"   ,DataTypes.STRING  ,(short)50 );
				ds.addColumn("job"     ,DataTypes.STRING  ,(short)10 );
				ds.addColumn("mgr"     ,DataTypes.STRING  ,(short)10 );
				ds.addColumn("hiredate",DataTypes.STRING  ,(short)10 );
				ds.addColumn("sal"     ,DataTypes.STRING  ,(short)10 );
				ds.addColumn("comm"    ,DataTypes.STRING  ,(short)10 );
				ds.addColumn("deptno"  ,DataTypes.STRING  ,(short)10 );
				
				//다시 조회 row수만큼 반복하면서 63번에서 추가될 공간을 먼저 확보한다.
				while(rs.next())
				{
					int row = ds.newRow();
					//65번부터 읽어온 정보를 dataset안에 초기화 해준다.
					ds.set(row ,"empno"       ,rs.getString("empno"));
					ds.set(row ,"ename"       ,rs.getString("ename"));
					ds.set(row ,"job"         ,rs.getString("job"));
					ds.set(row ,"mgr"         ,rs.getString("mgr"));
					ds.set(row ,"hiredate"    ,rs.getString("hiredate"));
					ds.set(row ,"sal"         ,rs.getString("sal"));
					ds.set(row ,"comm"        ,rs.getString("comm"));
					ds.set(row ,"deptno"      ,rs.getString("deptno"));
				}
				  
				// #1 dataset -> PlatformData
				out_pData.addDataSet(ds);

				// #2 dataset -> PlatformData
				//DataSetList dsList = out_pData.getDataSetList();
				//dsList.add(ds);

				nErrorCode  = 0;
				strErrorMsg = "SUCC";
				
			} catch (SQLException e) {
				nErrorCode = -1;
				strErrorMsg = e.getMessage();
			}    
			
			/******** JDBC Close ********/
			if ( stmt != null ) try { stmt.close(); } catch (Exception e) {nErrorCode = -1; strErrorMsg = e.getMessage();}
			if ( conn != null ) try { conn.close(); } catch (Exception e) {nErrorCode = -1; strErrorMsg = e.getMessage();}
					
			} catch (Throwable th) {
				nErrorCode = -1;
				strErrorMsg = th.getMessage();
		}

		VariableList varList = out_pData.getVariableList();
		varList.add("ErrorCode", nErrorCode);
		varList.add("ErrorMsg" , strErrorMsg);

		HttpPlatformResponse pRes = new HttpPlatformResponse(res, PlatformType.CONTENT_TYPE_XML, "utf-8");
		pRes.setData(out_pData);

		// Send data
		try {
			pRes.sendData();
		} catch (PlatformException e) {
			e.printStackTrace();
		}
		logger.info("doPost호출성공");
	}
}
```

