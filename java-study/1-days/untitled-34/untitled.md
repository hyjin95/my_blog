# Member/login.test : Spring, Java, MyBatis

### 사용 src, xml

![](../../../.gitbook/assets/t%20%281%29.png)

## xml

### 코드 : web.xml

```markup
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://java.sun.com/xml/ns/javaee" xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd" id="WebApp_ID" version="3.0">
	<context-param>
		<param-name>log4jConfigLocation</param-name>
		<param-value>/WEB-INF/classes/log4j.properties</param-value>
	</context-param>
	<!-- The definition of the Root Spring Container shared by all Servlets and Filters -->
	<context-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>
			/WEB-INF/spring-service.xml
		    ,/WEB-INF/spring-data.xml	
		</param-value>
	</context-param>
	
	<!-- Creates the Spring Container shared by all Servlets and Filters -->
	<listener>
		<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
	</listener>  
	<servlet>
		<servlet-name>appServlet</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
		<init-param>
			<param-name>contextConfigLocation</param-name>
			<param-value>/WEB-INF/spring-servlet.xml</param-value>
		</init-param>
		<load-on-startup>1</load-on-startup>
	</servlet>
		
	<servlet-mapping>
		<servlet-name>appServlet</servlet-name>
		<url-pattern>*.test</url-pattern>
	</servlet-mapping>
</web-app>
```

* .test로 끝나는 모든 url요청을 인터셉트해 DispatcherServlet에게 매핑해준다. 
* &lt;listener&gt;태그의 ContextLodaderListener클래스가 spring-service.xml 과 spring-data.xml을 읽을 수 있게 해준다.
* 8번의 &lt;context-param&gt;태그는 서버 기동시 한번 읽고 유지된다.
* 23번의 &lt;init-param&gt;태그는 요청이 들어올때마다 읽는다. - 요청 발생 시 마다 spring-servlet.xml을 읽는다.

### 코드 : spring-servlet.xml

```markup
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:mvc="http://www.springframework.org/schema/mvc"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-3.2.xsd
		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-3.2.xsd
		http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc-3.2.xsd">
	<bean id="member-controller" class="com.spring.mvc1.MemberController">
		<property name="methodNameResolver" ref="propertiesPathNameResolver"/>
		<property name="memberLogic" ref="member-logic"/>
	</bean>
	<bean id="simpleUrlHandlerMapping" class="org.springframework.web.servlet.handler.SimpleUrlHandlerMapping">
		<property name="mappings">
			<props>
				<prop key="/member/login.test">member-controller</prop>
			</props>
		</property>
	</bean>      
	<bean id="propertiesPathNameResolver" class="org.springframework.web.servlet.mvc.multiaction.PropertiesMethodNameResolver">
		<property name="mappings">
			<props>
				<prop key="/member/login.test">login</prop>
			</props>
		</property>
	</bean> 
</beans>
```

* DispatcherServlet이 받은 url로 클래스와 메서드를 찾는 곳이다.
* 13번에서 spring이 제공하는 simpleUrlHandlerMapping클래스로 등록된 클래스 이름을 찾아 9번의 클래스 경로를 읽는다. 
* 10번에서 methodNameResolver 의 propertiexPathNameResolver로 20번에서 해당 메서드를 찾는다.
* 11번 property코드로 memberLogic을 등록해 필요한 때에 주입받을 수 있게 한다.

### 코드 : spring-service.xml

```markup
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
	<bean id="member-logic" class="com.spring.mvc1.MemberLogic">
		<property name="sqlMemberDao" ref="sql-member-dao"/>
	</bean>
</beans>
```

* spring-servlet.xml의 ref member-logic으로 5번에서 MemberLoigc클래스를 찾을 수 있다.
* 6번의 property에서 sqlMemberDao클래스를 주입받을 수 있도록 한다.

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
	
	<!-- myBatis를 사용할 수 있도록 spring에서 sqlSessionFactory를 제공한다. -->
	<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean"><!-- driver class이름 -->
		<property name="configLocation" value="WEB-INF/mybatis-config.xml"/>
		<property name="dataSource" ref="data-source-target"/>
	</bean>
	
	<!-- myBatis를 사용할 수 있도록 spring에서 sqlSessionTemplate=sqlSesion를 제공한다. 위 bean과 의존관계에 있다. -->
	<bean id="sqlSessionTemplate" class="org.mybatis.spring.SqlSessionTemplate">
		<constructor-arg index="0" ref="sqlSessionFactory"/>
	</bean>
	
	<bean id="sql-member-dao" class="com.spring.mvc1.SqlMemberDao">
		<property name="sqlSessionTemplate" ref="sqlSessionTemplate"/>
	</bean>			
</beans>
```

* DB와의 연동에 필요한, Dao클래스에서 사용되는 xml이다.
* 이 xml문서는 서버 기동시 서버가 읽고 계속 유지한다.
* 31-33번은 SqlMemberDao에서 sqlSessionTemplate를 필요할떄 주입받을 수 있게 하기 위함이다.
* 27번의 SqlSessionTemplate클래스는 sql문을 요청하는 클래스로, 28번 태그에서 연결통로를 주입받는다.
* 21번의 sqlSessionFactoryBean은 연결통로를 제공해주는 클래스이다. 22번에서 MyBatis Layer의 xml과 연결되어 있고, data-source-target을 주입받아 DB와의 연결을 한다.

### 코드 : mybatis-config.xml

```markup
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration
 PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
 "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
	<mappers>
		<mapper resource="oracle/mybatis/member.xml"/>
	</mappers>
</configuration>
```

* MyBatis Layer의 sql문 xml 매핑 xml이다.

### 코드 : member.xml

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

* 프로시저를 호출하는 sql문으로, 프로시저를 활용하기떄문에 리턴타입은 필요없다. 파라미터로 받은 map에 응답을 출력한다.
* spatamentType="CALLABLE"이 있어야 필요할때 자동으로 호출된다.

## JAVA

### 코드 : MemberController.java

```java
package com.spring.mvc1;

import java.util.HashMap;
import java.util.Map;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.apache.log4j.Logger;
import org.springframework.web.servlet.ModelAndView;
import org.springframework.web.servlet.mvc.multiaction.MultiActionController;

public class MemberController extends MultiActionController {
	Logger logger = Logger.getLogger(MemberController.class);
	
	//전체 인스턴스화가 끝나면 순 제어 이므로 null로 두어 외부에서 주입받을 준비를 한다.
	private MemberLogic memberLogic = null;
	
	//여기 setter메서드를 통해 필요한 순간에 객체를 주입받는다.
	public void setMemberLogic(MemberLogic memberLogic) {
		this.memberLogic = memberLogic;
	}
	
	public ModelAndView login(HttpServletRequest req, HttpServletResponse res) {
		logger.info("Controller-login 호출 성공");
		
		Map<String, Object> pMap = new HashMap<>();
		pMap.put("mem_id", req.getParameter("mem_id"));
		pMap.put("mem_pw", req.getParameter("mem_pw"));
		memberLogic.login(pMap);
		
		ModelAndView mav = new ModelAndView();
		return mav;
	}
}
```

* Java 코드로 작성된 Controller클래스, Spring에서 제공하는 MultiActionController를 상속받는다. - MultiActionController를 상속받으면 여러 요청을 한 Controller에서 메서드를 구현해 처리할 수 있게된다. SimpleUrlController를 사용한다면 요청에 대해 각각의 업무 Controller를\(Controller인터페이스나 AbstractController추상클래스를 상속하는\) 구현해야한다.
* MemberLogic의 인스턴스변수를 private로 선언해 놓고, setter메서드를 구현한다. - 필요한 순간에 객체를 주입받아 원본을 사용하기 위함 -- spring-servlet.xml
* login메서드 파라미터의  req, res는 DispatcherServlet이 주입해준다.

### 코드 : MemberLogic.java

```java
package com.spring.mvc1;

import java.util.HashMap;
import java.util.Map;

import org.apache.log4j.Logger;

public class MemberLogic {
	Logger logger = Logger.getLogger(MemberLogic.class);
	
	private SqlMemberDao sqlMemberDao = null;
	
	//원본 주입받아 사용하기
	public void setSqlMemberDao(SqlMemberDao sqlMemberDao) {
		this.sqlMemberDao = sqlMemberDao;
	}
	
	public String login(Map<String, Object> pMap) {
		logger.info("Logic-login 호출성공");
		String mem_name = null;
		mem_name = sqlMemberDao.login(pMap);
		logger.info("오라클에서 꺼낸이름 : "+mem_name);
		return mem_name;
	}
}
```

* 11번에서 Dao의 인스턴스변수를 생성하고, setter메서드를 통해 필요할때 주입받는다. -- spring-service.xml

### 코드 : SqlMemberDao.java

```java
package com.spring.mvc1;

import java.util.Map;

import org.apache.log4j.Logger;
import org.mybatis.spring.SqlSessionTemplate;

public class SqlMemberDao {
	Logger logger = Logger.getLogger(SqlMemberDao.class);
	
	private SqlSessionTemplate sqlSessionTemplate = null;
	
	public void setSqlSessionTemplate(SqlSessionTemplate sqlSessionTemplate) {
		this.sqlSessionTemplate = sqlSessionTemplate;
	}

	//프로시저의 파라미터 map은 파라미터이면서 result임을 인식하자
	public String login(Map<String,Object> pmap){
		logger.info("Dao-login 호출성공");
		String mem_name = null;
		try {
			sqlSessionTemplate.selectOne("proc_ajaxLogin",pmap);
			mem_name = pmap.get("msg").toString();
		} catch (Exception e) {
			e.printStackTrace();
		}
		return mem_name;
	}
}
```

* 11번에서 SqlSessionTemplate인스턴스변수를 생성해놓고, setter메서드를 통해 필요할때 주입받아 사용한다. --spring-data.xml
* DB연결 Driver와 연결통로가 xml에 작성되어 있기때문에 sqlSessionTemplate만 주입받아 바로 sql문을 호출 하면된다. - MyBatis : selectOne, selectList, isert, update, delete, ....

## Log4j

### 코드 : log4j.properties

```bash
#log4j.properties
log4j.rootCategory=info, stdout, file
log4j.debug=false
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.ImmediateFlush=true
log4j.appender.stdout.Target=System.err

log4j.appender.stdout.layout.ConversionPattern=[%d] [%p] (%13F:%L) %3x - %m%n


log4j.appender.file.DatePattern = '.'yyyy-MM-dd

#emp.xml이나 dept.xml, zipcode.xml의 namespace이름을 등록한다.
log4j.logger.oracle.mybatis.MemberMapper = TRACE

log4j.appender.file.layout=org.apache.log4j.PatternLayout
log4j.appender.file.layout.ConversionPattern=[%d] [%p] (%13F:%L) %3x - %m%n

log4j.logger.java.sql.Connection=INFO
log4j.logger.java.sql.Statement=INFO
log4j.logger.java.sql.PreparedStatement=INFO
log4j.logger.java.sql.ResultSet=INFO
```

