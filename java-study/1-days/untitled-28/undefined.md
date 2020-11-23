# 로그인 프로시저 구현 : 비동기통신

## 코드 :  View

### index.jsp

```markup
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<%@ include file="/common/easyUI_common.jsp" %>
<style type="text/css">
	div#d_login{
		border: 1px solid gray;
	}
</style>
<script type="text/javascript">
	function login(){
		var u_id = $("#tb_id").val();
		var u_pw = $("#tb_pw").val();
		$.ajax({
			 url:'./login.mem?work=login&mem_id='+u_id+'&mem_pw='+u_pw+'&'+new Date().getTime()
			,type:'get'
			,success:function(data){
				$("#d_login").text("");
				var imsi='';            
	            imsi+='<table border="0">';
	            imsi+='<tr><td height="30px">';
	            imsi+=data;
	            imsi+='</td></tr>';
	            imsi+='<tr><td>';
	            imsi+='<a id="btn_logout" class="easyui-linkbutton" onclick="logout()">로그아웃</a>';
	            imsi+='<a id="btn_member" class="easyui-linkbutton" onclick="memberUpdate()">정보수정</a>';            
	            imsi+='</td></tr>';
	            imsi+='</table>';
				$("#d_login").html(imsi);
			}
		});
	}/////////////////////end of login
</script>
</head>
<body>
<table align="center" width="1000px" height="550px">
	<tr>
		<td>
			<table>
<!------------------------- [[ menu.jsp 시작]] ------------------------------>
								<td width="200px" height="470px">
									<%@ include file="./menu.jsp" %>
								</td>
<!------------------------- [[ menu.jsp 종료]] ------------------------------>
			</table>
		</td>
	</tr>
</table>
</body>
</html>
```

### menu.jsp

```markup
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<table width="100%"  height="100%" border="1" borderColor="orange">
	<tr>
		<td align="center" valign="top">
			<table>
<!-- 로그인 화면 시작 -->		
				<tr>
					<td width="190px" height="75px" align="left">
						<div id="d_login">	
							<table border="0">
								<tr>
									<td><input id="tb_id" class="easyui-textbox" data-options="iconCls:'icon-man', prompt:'아이디'" style="width:130px"></td>
									<td rowspan="2"><a id="btn_login" class="easyui-linkbutton" onclick="login()">로그인</a></td>
									</tr>
								<tr>
									<td><input id="tb_pw" class="easyui-textbox" data-options="iconCls:'icon-lock', prompt:'비밀번호'" style="width:130px"></td>
								</tr>
							</table>
						</div>
					</td>
				</tr>
<!-- 로그인 화면 종료 -->					
			</table>
		</td>
	</tr>
</table>
```

## 코드 : Servlet

### FrontController.java

```java
package com.ajax.news;

public class FrontController extends HttpServlet {
	Logger logger = Logger.getLogger(FrontController.class);
	
	//forward, sendRedirect
	//Q1. doGet과 doPost 모두 리턴타입이 void임에도 불구하고 처리된 결과를 jsp페이지에 유지해야한다.
	//어떻게 해야할까?
	//A. view.forward(req, res);
	//단, forward이전에 req.setAttribute("이름",값);으로 request에게 data를 담아 유지해야한다.
	
	//여기서는 사용할 수 없다. 서버로부터 req, res객체를 주입받기 이전이기때문에 사용될 수 없다.
	//String work = req.getParameter();

	@Override
	public void doGet(HttpServletRequest req, HttpServletResponse res) 
		throws ServletException, IOException{
			logger.info("doGet 호출성공");
			String work = req.getParameter("work");
			MemberLogic memLogic = new MemberLogic();
			
			//url을 통해 업무에 대한 경우의 수를 나눈다. = if문
			if("login".equals(work)) {
				Map<String, Object> pMap = new HashMap<>();
				pMap.put("mem_id", req.getParameter("mem_id"));
				pMap.put("mem_pw", req.getParameter("mem_pw"));
				String mem_name = memLogic.login(pMap);
				logger.info("서블릿의 mem_name : "+mem_name);
				//logic->dao해서 가져온 값을 쿠키나 세션이 담아야한다.
				req.setAttribute("mem_name", mem_name);
				RequestDispatcher view = req.getRequestDispatcher("./loginAction2.jsp");
				view.forward(req, res);
			}			
}
```

## 코드 : java

### MemberLogic.java

```java
package com.ajax.member;

import java.util.Map;

import org.apache.log4j.Logger;

/*
 * Logic클래스의 역할은 업무에 대한 프로세스(멀티[select->insert, select->update, select->delete], 다중처리[여러 DB연동])를 이해하고 있으면서 어떤 테이블에서 조회할 것인가
 * 그 조회한 결과를 어떤 테이블에 추가할 것인가 등을 판단하고 결졍하는 일을 담당한다.
 * 하나의 로직, 하나의 메서드안에서 DAO에 위치한 여러개의 메서드를 한번에 요청할 수 있다.=다중처리가 가능하다. 트랜잭션의 대상이된다.
 * 그러므로 로직에서 트랜잭션처리가 이뤄져야한다. commit과 rollback의 메서드를 호출하는 시점도 로직이 되어야 한다.
 * 이렇게 재사용되어야 하는 코드를 분리하기위해 AOP API가 필요한것이다.--분리하는 기능을 갖는 API
 * AOP에 대한 F/W이 존재한다.
 * 자동 트랜잭션, 일괄 적용할 수 있도록 업무 시간을 절약한다.
 * con.setAutoCommit(false); --트랜잭션시작
 * int r1 =pstmt.executeUpdate();
 * int r2 =pstmt.executeUpdate();
 * if(r1==1 && r2==1) con.commit();//하나라도 0이면 rollback
 * con.setAutocommit(true); -- 트랜잭션종료, 길다 API로 만들어보자
 * 직접 오라클과 연동하지 않아야한다. Dao가 한다.
 * 그래야 한 업무단위에 대해 트랜잭션을 할 수 있고 재사용성이 좋아지므로 
 * 그래서 독립적이어야한다.
 * 그래서 결합도가 낮아지고 재사용성이 높아진다.
 */
public class MemberLogic {
	Logger logger = Logger.getLogger(MemberLogic.class);
	
	//오라클과 연동하는 일을 담당하는 클래스 객체 주입이 필요하다.
	//하나 뒤의 클래스를 하나 앞의 클래스에 인스턴스화(DI)해야한다.
	SqlMemberDao smDao = null;//선언시 null -> 22번
	
	public String login(Map<String, Object> pMap) {
		logger.info("login 호출성공");
		String mem_name = null;
		smDao = new SqlMemberDao();//필요한 시점에 객체주입받기
		mem_name = smDao.login(pMap);
		logger.info("오라클에서 꺼낸이름 : "+mem_name);
		return mem_name;
	}
}
```

### SqlMemberDao.java

```java
package com.ajax.member;

import java.io.Reader;
import java.sql.SQLException;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.apache.log4j.Logger;

import com.util.MyBatisCommonFactory;
//클래스 조립해보기
//MyBatis에서 프로시저 사용 단위테스트 코딩전개해보기
public class SqlMemberDao {
	Logger logger = Logger.getLogger(SqlMemberDao.class);
	String resource = "com/util/Configuration.xml";
	SqlSessionFactory sqlMapper = null;
	
	//프로시저의 파라미터 map은 파라미터이면서 result임을 인식하자
	public String login(Map<String,Object> pmap){
		String mem_name = null;
		SqlSession session = null;		
		try {
			Reader reader = Resources.getResourceAsReader(resource);
			sqlMapper = new SqlSessionFactoryBuilder().build(reader,"development");
			logger.info("sqlMapper"+sqlMapper);
			//아래에서 생성한 session은 insert(), update(), delete(), selectOne, selectList, selectMap을 사용하기 위함이다.
			session = sqlMapper.openSession();
			session.selectOne("proc_ajaxLogin",pmap);//파라미터이면서 result이다. In과 OUT을 둘다 받아준다.
			logger.info("이름 : "+pmap.get("msg"));
			mem_name = pmap.get("msg").toString();
			//세션을 닫아주지 않으면 오라클 서버에서 세션 수가 허용치를 넘으면 강제로 꺼진다. 
			session.close();
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			session.close();
		}
		return mem_name;
	}
	/*
	public static void main(String args[]) { 단위테스트
		SqlMemberDao smDao = new SqlMemberDao();
		Map<String, Object> pmap = new HashMap<>();
		pmap.put("mem_id", "test");
		pmap.put("mem_pw", "123");
		smDao.login(pmap);
	}*/
}
```

### member.xml

```markup
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="oracle.mybatis.MemberMapper">
	<select id="proc_ajaxLogin" parameterType="map" statementType="CALLABLE"><!-- 프로시저 호출이기때문에 리턴값이 없어 리턴타입이 필요없다 -->
		{call proc_ajaxLogin( #{mem_id, mode=IN, jdbcType=VARCHAR, javaType=java.lang.String}
							 ,#{mem_pw, mode=IN, jdbcType=VARCHAR, javaType=java.lang.String}
							 ,#{msg, mode=OUT, jdbcType=VARCHAR, javaType=java.lang.String}
							 ,#{status, mode=OUT, jdbcType=VARCHAR, javaType=java.lang.String}
		)}
	 </select>
</mapper>
```

* Configuration문서에 mapping되어야 한다.

