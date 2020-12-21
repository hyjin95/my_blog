# Spring : 계층형 게시판

![](../../../.gitbook/assets/1%20%2894%29.png)

![](../../../.gitbook/assets/.png%20%2850%29.png)

![](../../../.gitbook/assets/2%20%2871%29.png)

## JAVA

### 코드 : BoardController.java

```java
public class BoardController extends MultiActionController {//spring제공 클래스
	
	private BoardLogic boardLogic = null;

	public void setBoardLogic(BoardLogic boardLogic) {
		this.boardLogic = boardLogic;
	}

	public ModelAndView boardList(HttpServletRequest req, HttpServletResponse res)//viewResolver를 사용한다.
			throws Exception{
		logger.info("controller - boardList호출성공");
		List<Map<String,Object>> boardList = null;
		boardList = boardLogic.boardList(null);
		ModelAndView mav = new ModelAndView();
		mav.addObject("boardList", boardList);
		mav.setViewName("board/boardList");
		return mav;
	}
}
```

* pMap에 파라미터를 받아 조건값으로 넘겨줘야 하지만 일단 파라미터 없이 호출 테스트 하기 위해 null을 넘긴다.

### 코드 : BoardLogic.java

```java
public class BoardLogic {
	
	public SqlBoardMDao sqlBoardMDao = null;
	
	public void setSqlBoardMDao(SqlBoardMDao sqlBoardMDao) {
		this.sqlBoardMDao = sqlBoardMDao;
	}

	public SqlBoardDDao sqlBoardDDao = null;
	
	public void setSqlBoardDDao(SqlBoardDDao sqlBoardDDao) {
		this.sqlBoardDDao = sqlBoardDDao;
	}	

	public List<Map<String,Object>> boardList(Map<String, Object> pMap) {
		logger.info("Logic - boardList 호출 성공 : "+pMap);
		List<Map<String,Object>> bList = null;
		bList = sqlBoardMDao.boardList(pMap);
		return bList;
	}
}
```

* insert나 update, delete와 같이 트랜잭션 처리와 상관없는 조회 업무이기때문에 MasterDao만으로 처리한다.

### 코드 : BoardMDao.java

```java
public class SqlBoardMDao {
	
	private SqlSessionTemplate sqlSessionTemplate = null;

	public void setSqlSessionTemplate(SqlSessionTemplate sqlSessionTemplate) {
		this.sqlSessionTemplate = sqlSessionTemplate;
	}
  public List<Map<String, Object>> boardList(Map<String, Object> pMap) {
		logger.info("MDao - boardList 호출 성공 : "+pMap);
		List<Map<String,Object>> bList = null;
		bList = sqlSessionTemplate.selectList("boardList",pMap);
		return bList;
	}
}
```

## myBatis-xml, log4j.properties

### 코드 : board.xml

```markup
<mapper namespace="oracle.mybatis.BoardMapper">
	<select id="boardList" parameterType="map" resultType="map"><!-- bs테이블은 null일 수 있으므로 nullpointer방지로 NVL구문 사용 -->		   
		SELECT 
			   bm.bm_no, bm.bm_title, bm.bm_writer, bm.bm_content, bm.bm_email
			   ,bm.bm_pw, bm.bm_group, bm.bm_pos, bm.bm_step, bm.bm_hit
			   ,NVL(bs.bs_file,'') bs_file, NVL(bs.bs_size,0) bs_size
		  FROM board_master_t bm right outer join board_sub_t bs
		    ON bm.bm_no = bs.bm_no
		 WHERE bm_title LIKE '%'||'목'||'%'
	</select>
</mapper>
```

### 코드 : log4j.properties

```markup
log4j.logger.oracle.mybatis.BoardMapper=TRACE
```

## XML

### 코드 : mybatis-config.xml

```markup
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
 PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
 "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
	<mappers>
	 	<mapper resource="oracle/mybatis/board.xml"/>
	</mappers>
</configuration>
```

### 코드 : spring-servlet.xml

```markup
	<bean id="board-controller" class="com.board.BoardController">
		<property name="methodNameResolver" ref="board-resolver"/>
		<property name="boardLogic" ref="board-logic"/>
	</bean>
	
	<!-- 해당하는 요청 URL에 대한 메서드 이름을 매칭하는 곳 -->
	<bean id="board-resolver" class="org.springframework.web.servlet.mvc.multiaction.PropertiesMethodNameResolver">
		<property name="mappings"><!-- setter메서드의 이름과 동일해야만한다. -->
			<props>
				<prop key="/board/boardList.sp">boardList</prop>
			</props>
		</property>
	</bean> 
	
	<!-- IoC를 위한 컨트롤계층 선언 및 등록 -->
	<bean id="simpleUrlHandlerMapping" class="org.springframework.web.servlet.handler.SimpleUrlHandlerMapping">
		<property name="mappings">
			<props>
				<prop key="/board/boardList.sp">board-controller</prop>
			</props>
		</property>
	</bean>       
	
	<!-- viewResolver추가 -->
	<bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<property name="prefix" value="/WEB-INF/views/"></property>
		<property name="suffix" value=".jsp"></property>
	</bean>
```

### 코드 : spring-service.xml

```markup
	<bean id="board-logic" class="com.board.BoardLogic">
		<property name="sqlBoardMDao" ref="sql-board-mdao"/>
		<property name="sqlBoardDDao" ref="sql-board-ddao"/>
	</bean>
```

### 코드 : spring-data.xml

```markup
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
	<bean id="data-source-target" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
		<property name="driverClassName">
			<value>oracle.jdbc.driver.OracleDriver</value>
		</property>
		<property name="url">
			<value>jdbc:oracle:thin:@192.168.0.187:1521:orcl11</value>
		</property>
		<property name="username">
			<value>scott</value>
		</property>
		<property name="password">
			<value>tiger</value>
		</property>
	</bean>
	
	<!-- myBatis를 사용할 수 있도록 spring에서 sqlSessionFactory를 제공 -->
	<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean"><!-- driver class이름 -->
		<property name="configLocation" value="WEB-INF/mybatis-config.xml"/>
		<property name="dataSource" ref="data-source-target"/>
	</bean>
	
	<!-- myBatis를 사용할 수 있도록 spring에서 sqlSessionTemplate=sqlSesion를 제공 -->
	<bean id="sqlSessionTemplate" class="org.mybatis.spring.SqlSessionTemplate">
		<constructor-arg index="0" ref="sqlSessionFactory"/>
	</bean>
	
	<bean id="sql-board-mdao" class="com.board.SqlBoardMDao">
		<property name="sqlSessionTemplate" ref="sqlSessionTemplate"/>
	</bean>		
		
	<bean id="sql-board-ddao" class="com.board.SqlBoardDDao">
		<property name="sqlSessionTemplate" ref="sqlSessionTemplate"/>
	</bean>	
			
</beans>
```

## 화면 : easyUI

### 코드 : boardList.jsp

```markup
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ page import = "java.util.List, java.util.Map, java.util.HashMap" %>
<%
	int tot = 0;
	List<Map<String,Object>> boardList = null;
	boardList = (List<Map<String,Object>>)request.getAttribute("boardList");
	if(boardList != null){
		tot = boardList.size();
	}
%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>게시판 목록[WEB-INF/views]</title>
<%@ include file="/common/easyUI_common.jsp" %>
</head>
<body>
<script type="text/javascript">
	
</script>
게시판 목록
<!-- 
======================== [[글목록 화면 시작]] =========================
JEasyUI의 DataGrid API를 활용하여 작성
1)익스프레션을 이용해서 화면 처리
  :tr, td태그를 직접 작성해서 처리하는 방식
2)json포맷으로 처리해서 매핑
  :field명만 맞춰주면 자동으로 매핑
 -->
 <table id="dg_board" title="글목록" style="width:950px;height:500px" class="easyui-datagrid" data-options="singleSelect:'true',toolbar:'#tb_search,#tb_board',footer:'#pn_board'">
    <!--헤더부분 추가 -->
    <thead>
       <tr>
          <th data-options="field:'BM_TITLE',width:'350px'">제목</th>
            <th data-options="field:'BM_WRITER',width:'100px'">작성자</th>
            <th data-options="field:'BM_DATE',width:'110px'">작성일</th>
            <th data-options="field:'BS_FILE',width:'280px'">첨부파일</th>
            <th data-options="field:'BM_HIT',width:'100px'">조회수</th>
       </tr>
    </thead>
    <!--데이터 출력 영역  -->
    <tbody>
    <!---================== 조회결과가 없는 경우 ======================= -->
    <%
    	if(tot==0){
    %>
    	<tr>
    		<td colspan="5">&nbsp;조회 결과가 없습니다.</td>
    	</tr>
    <%	
    	}
    %>
    	
    <!---================== 조회결과가 있는 경우 ======================= -->    	
    <%
    	if(tot > 0){
    		for(int i=0;i<tot;i++){ 		   	
    %>
    	<tr>
            <td><%="제목" %></td>
            <td><%="작성자" %></td>
            <td><%="작성일" %></td>
            <td><%="첨부파일" %></td>
            <td><%="조회수" %></td>
       </tr>
     <%
    		}
    	}
    		
     %>
    </tbody>
 </table>   
</body>
</html>
```

## 결과 : url요청

![](../../../.gitbook/assets/1%20%2892%29.png)

* /board/boardList.sp

