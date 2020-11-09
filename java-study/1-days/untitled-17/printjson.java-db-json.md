# printJson.java - DB연동, JSON형식 출력하기

## printJson.java

```java
package com.util;

import java.io.IOException;
import java.io.PrintWriter;
import java.io.Reader;
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

//Servlet파일
public class PrintJson extends HttpServlet {
	
	Logger logger = Logger.getLogger(PrintJson.class);
	//xml문서가 물리적으로 어디에 있는지를 알아야 한다.
	String resource ="com/util/Configuration.xml";
	//myBatis에서 지원하는 클래스로
	SqlSessionFactory sqlMapper = null;
	
	/******************************************************************
	 * 조회결과를 JSON포맷으로 내보내기 구현
	 * url-pattern:web.xml에 먼저 등록하기
	 * common/toJson.do를 pattern으로 사용해보기
	 ******************************************************************/
	public void doGet(HttpServletRequest req, HttpServletResponse res) 
	throws ServletException, IOException {
		
		logger.info("doGet 출력성공");
		PrintWriter out = res.getWriter();
		res.setContentType("application/json; charset=UTF-8");//mime타입으로 타입 JSON으로 지정, data가 어떤 언어인지 모르므로 charset까지 넣어준다.		
		List<Map<String, Object>>empList = null;
		SqlSession session = null;
		String imsi = null;
		try {
			//물리적으로 떨어져있는 소스에서 필요한 정보를 읽어와야한다. Reader<->Writer
			//Reader = 문서를 읽는데 필요한 클래스
			Reader reader = Resources.getResourceAsReader(resource);
			//build클래스로 sqlSessionFactory를 인스턴스할 수 있다.
			sqlMapper = new SqlSessionFactoryBuilder().build(reader);
			logger.info("before->"+sqlMapper);
			session = sqlMapper.openSession();
			logger.info("after>"+session);
			Map<String,Object> target = new HashMap<>();
			empList = session.selectList("getEmpList",target);			
			session.close();			
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			session.close();
		}
		if(empList==null) {//조회결과가 없을 떄 진행
			empList = new ArrayList<>();
		}else {//조회결과가 있을 때 진행
			Gson g = new Gson();
			imsi = g.toJson(empList);
		}
		out.print(imsi);
		//out.print("[{deptno:10}]");//JSON형식이 [ ]로인데 String값이 들어가 출력시 모두 String [ ]덩어리로 나와버린다.주석으로 막아야 json형식으로 data출력됨
	}
}
```

## Web.xml

```markup
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd" id="WebApp_ID" version="4.0">
<!-- 
sever.xml은 톰캣서버가 기동할떄 디폴트, 기본으로 읽게되는데
xml의 규칙 내에서 포트번호를 결정하고 프로젝트를 배치한다. 
-->
<!-- log4j 환경파일 등록하기 서버가 기동된 동안에는 계속 유지된다. -->
	<context-param>
		<param-name>log4jConfigLocation</param-name><!-- 객체주입 -->
		<param-value>/WEB-INF/classes/log4j.properties</param-value><!-- 톰캣서버가 읽을 수 있게 한다. -->
	</context-param>
<!-- DD파일(Deployment Discriptor) = 배치서술자 -->
	<servlet>
		<servlet-name>testMgr</servlet-name>
		<servlet-class>web.mvc.TestController</servlet-class>
	</servlet>
	<servlet-mapping>
		<servlet-name>testMgr</servlet-name>
		<url-pattern>/test/test.do</url-pattern><!-- url -->
		<!-- 웹서비스를 할때에 자바를 인스턴스화할 수 없으므로 url로 접근한다. 이를 위해 xml dd파일에 url을 생성해야한다. -->
	</servlet-mapping>
	
	<servlet>
		<servlet-name>commonJSON</servlet-name>
		<servlet-class>com.util.PrintJson</servlet-class>
	</servlet>
	<servlet-mapping>
		<servlet-name>commonJSON</servlet-name>
		<url-pattern>/common/toJson.do</url-pattern><!-- url -->
		<!-- 웹서비스를 할때에 자바를 인스턴스화할 수 없으므로 url로 접근한다. 이를 위해 xml dd파일에 url을 생성해야한다. -->
	</servlet-mapping>
</web-app>
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
	 WHERE 1=1
</select>
</mapper>
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

## 실행

![](../../../.gitbook/assets/1%20%2852%29.png)

