# step1, getEmpList.jsp - JSON, JSP

## step1 - Atype, html

![combobox](../../.gitbook/assets/1%20%2855%29.png)

![&#xAC1C;&#xBC1C;&#xBD80; &#xC120;&#xD0DD;&#xC2DC;](../../.gitbook/assets/2%20%2843%29.png)

```markup
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>combobox step1 : A-html</title>
	<link rel="stylesheet" type="text/css" href="https://www.jeasyui.com/easyui/themes/default/easyui.css">
    <link rel="stylesheet" type="text/css" href="https://www.jeasyui.com/easyui/themes/icon.css">
    <link rel="stylesheet" type="text/css" href="https://www.jeasyui.com/easyui/themes/color.css">
    <link rel="stylesheet" type="text/css" href="https://www.jeasyui.com/easyui/demo/demo.css">
    <script type="text/javascript" src="https://www.jeasyui.com/easyui/jquery.min.js"></script>
    <script type="text/javascript" src="https://www.jeasyui.com/easyui/jquery.easyui.min.js"></script>
</head>
<body>
<input id="cc1" class="easyui-combobox" data-options="
        valueField: 'DEPTNO',
        textField: 'DNAME',
        url: '../datagrid/dept.json',
        onSelect: function(rec){
            var url = '../getEmpList.jsp?deptno='+rec.DEPTNO;
        }">
</body>
</html>
```

## getEmpList.jsp

```java
<%@ page language="java" contentType="application/json; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ page import="java.sql.*, java.util.*" %>
<%@ page import="com.google.gson.Gson" %>

<%
	//코드 입양시 더 집중하기, 부분적으로 수정해야한다. 연과되는 것들 모두 수정이 이루어져야 한다.
	//위치파악, 앞뒤에 표시하기
	
	//json : list, map 필요
	List<Map<String, Object>> deptList = new ArrayList<>();//n개 row를 담기 위함
	Map<String, Object> rmap = null;//한개 row를 담기 위함
		
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
			//테이블 이름과 컬럼명반 바꾼다. 반복되는 나머지 코드는 줄인다 -- myBatis를 사용하는 이유
			String SQL;//쿼리문 담기
			SQL  = "SELECT deptno \n" +
				   "     , dname  \n" +
				   "     , loc    \n" +
				   "  FROM dept   \n" ;
			//" WHERE 1=1     \n" ; - 전체조회라면 필요없다.					
			rs = stmt.executeQuery(SQL);//오라클에게 DML문 처리 요청 모드
			
			//xml : 화면에 매칭(id:ds_emp)할 데이터셋 자바에서 초기화 하기 초기화 = 선언->인스턴스->값 부여
			//ds.addColumn("empno"   ,DataTypes.STRING  ,(short)10 );
			//ds.addColumn("ename"   ,DataTypes.STRING  ,(short)50 );
	
			//위의 코드는 xml타입일때 사용하는 코드이므로 json을 사용하는 지금은 필요가 없다.
			//json : 오라클에 있는 컬럼을 모두 꺼내 자바코드로 옮긴다.
			
			while(rs.next())
			{
				//int row = ds.newRow();//xml : 공간확보.
				//ds.set(row ,"empno"  ,rs.getString("empno"));
				//ds.set(row ,"ename"  ,rs.getString("ename"));
				//위의 코드는 xml타입일때 사용하는 코드이므로 json을 사용하는 지금은 필요가 없다., rs.getString만 사용
				rmap = new HashMap<>();
				rmap.put("deptno",rs.getInt("deptno"));
				rmap.put("dname" ,rs.getString("dname"));
				rmap.put("loc"   ,rs.getString("loc"));
				deptList.add(rmap);
			}/////////////////////////////end of while///////////////////////////////
			
		} catch (SQLException se) {
			out.print("SQLException"+se.toString());
		} 
		/******** JDBC Close ********/
		if ( stmt != null ) try { stmt.close(); } catch (Exception e) {}
		if ( conn != null ) try { conn.close(); } catch (Exception e) {}
	} catch(Exception e) {
		out.print("Exception"+e.toString());
	}////////////////////////////////////////end of outter try...catch
	Gson g = new Gson();//google제공 json API
	String temp = g.toJson(deptList);
	//out.print(deptList); JSON이 아닌 String덩어리로 나와버림
	out.print(temp);
%>
```



