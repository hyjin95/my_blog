# SQL 화면 없이 단위테스트 - 쿼리스트링

## getEmpList.jsp

```javascript
<%@ page language="java" contentType="application/json; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ page import="java.sql.*, java.util.*" %>
<%@ page import="com.google.gson.Gson" %>

<%
	//코드 입양시 더 집중하기, 부분적으로 수정해야한다. 연과되는 것들 모두 수정이 이루어져야 한다.
	//위치파악, 앞뒤에 표시하기
	
	//json : list, map 필요
	List<Map<String, Object>> empList = new ArrayList<>();//n개 row를 담기 위함
	Map<String, Object> rmap = null;//한개 row를 담기 위함
	
	//사용할 컬럼명, 값 받아오기, 듣기
	String cols = request.getParameter("cols");
	String keyword = request.getParameter("keyword");
	
	//쿼리문을 갱신하기 위해서는 원본이 바뀌지않는 String대신 StringBuilder를 사용한다.
	StringBuilder sql = new StringBuilder();
		
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
			sql.append("SELECT empno   ");
			sql.append("     , ename   ");
			sql.append("     , job     ");
			sql.append("     , mgr     ");
			sql.append("     , hiredate");
			sql.append("     , sal     ");
			sql.append("     , comm    ");
			sql.append("     , deptno  ");
			sql.append("  FROM emp     ");
			
			if(cols!=null && cols.length()>0 && keyword!=null && keyword.length()>0){//null, 공백체크
				sql.append(" WHERE "+cols+"="+keyword);
			}
							
			//rs = stmt.executeQuery(SQL);//작성한 쿼리문을 토드에서 테스트 해보고 싶을때
			rs = stmt.executeQuery(sql.toString());//오라클에게 DML문 처리 요청 모드
	
			//json : 오라클에 있는 컬럼을 모두 꺼내 자바코드로 옮긴다.			
			while(rs.next())
			{
				rmap = new HashMap<>();
				rmap.put("empno"    ,rs.getInt("empno"));
				rmap.put("ename"    ,rs.getString("ename"));
				rmap.put("job"      ,rs.getString("job"));
				rmap.put("mgr"      ,rs.getInt("mgr"));
				rmap.put("hiredate" ,rs.getString("hiredate"));
				rmap.put("sal"      ,rs.getString("sal"));
				rmap.put("comm"     ,rs.getString("comm"));
				rmap.put("deptno"   ,rs.getInt("deptno"));
				empList.add(rmap);
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
	String temp = g.toJson(empList);
	//out.print(deptList); JSON이 아닌 String덩어리로 나와버림
	out.print(temp);
%>
```

