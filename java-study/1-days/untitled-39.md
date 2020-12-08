---
description: 2020.12.08 - 80일차
---

# 80 Days -

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5 - Spring
* 사용 서버 - WAS : Tomcat

## POJO

### 웹 서비스 제공

* 주체 : 브라우저 - url주소 -&gt; http\(java처리\) -&gt; url로 업무분기\(if\)
* url : /프로젝트명/work name/업무이름.url-pattern
* java로 처리된다 = 안드로이드와의 연계가 가능하다.

### POJO 1-1

* url-pattern : \*.test
* url인터셉트 : FrontMVC1.java

### POJO 1-2

* url-pattern : \*.sp2
* url인터셉트 :  ActionServlet.java

### POJO 1-3

* url-pattern : \*.sp3
* url인터셉트 : ActionSupport.java
* ModelAndView3를 등록해 다중등록이 가능하도록 해보자.

### 공통점

* req, res에 의존적이다. - 없으면 사용할 수 없게 된다.\(sendRedirect, getAttribute, ...\)
* 페이지 이동에 대한 처리설계가 필요하다. - sendRedirect, forward

### 차이점

* url패턴이 다르다.
* return type 변경 - 표준 : void - settet, getter : ActionForward - ModelAndView\(1-2\)   jsp위치 주의 : webContent or WEB-INF - String\(1-3\)   redirect:xxx.jsp or forward:xxx.jsp   ' : '를 기준으로 문자열을 분리해 처리한다.   jsp위치 주의 : webContent or WEB-INF

## Spring

### 1-1

* url-pattern : \*.test
* url인터셉트 : DispatcherServlet -&gt; FrontMVC1.java

### 1-2

* url-pattern : \*.sp2
* url인터셉트 : DispatcherServlet -&gt; ActionServlet.java

### 1-3

* url-pattern : \*.sp3
* url인터셉트 : DispatcherServlet -&gt; ActionSupport.java
* ModelAndView3를 등록해 다중등록이 가능하도록 해보자

### SimpleUrlHandlerMapping

* 어느 Controller클래스와 매핑할지 xml또는 java에서 정해준다.
* SimpleUrlHandlerMapping의 &lt;property&gt;태그속성의 이름은 set메서드 이름과 일치해야 한다.

### java : java

* Controller - Logic - Dao

### java : xml

* Dao - SqlSessionTemplate

### xml : xml

* sqlSessionFactoryBean - sqlSessionTemplate

## HikariCP - DB, spring3

### 1. pom.xml에 dependency추가

```markup
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-jdbc</artifactId>
			<version>${org.springframework-version}</version>
		</dependency>
		
		<!-- HikariCP 3.3.1 -->
		<!-- https://mvnrepository.com/artifact/com.zaxxer/HikariCP -->
		<dependency>
		    <groupId>com.zaxxer</groupId>
		    <artifactId>HikariCP</artifactId>
		    <version>3.3.1</version>
		</dependency>
		
		<!-- myBatis 3.4.6 -->
		<!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
		<dependency>
		    <groupId>org.mybatis</groupId>
		    <artifactId>mybatis</artifactId>
		    <version>3.4.6</version>
		</dependency>
		
		<!-- myBatis-spring 1.3.3 -->
		<!-- https://mvnrepository.com/artifact/org.mybatis/mybatis-spring -->
		<dependency>
		    <groupId>org.mybatis</groupId>
		    <artifactId>mybatis-spring</artifactId>
		    <version>1.3.3</version>
		</dependency>
		
		<!-- Gson 2.8.6 -->
		<!-- https://mvnrepository.com/artifact/com.google.code.gson/gson -->
		<dependency>
		    <groupId>com.google.code.gson</groupId>
		    <artifactId>gson</artifactId>
		    <version>2.8.6</version>
		</dependency>
```

### 2. root-context.xml위치 설정

* root-context.xml에서 다른 의존성 주입도 받기위해 appServlet폴더로 이동한다. local에서 접근할 수 있도록 context.xml과 같은 경로상에 둔다.
* xml : xml으로의 객체 주입이 일어나야 한다.

### 코드 : root-context.xml

```markup
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans https://www.springframework.org/schema/beans/spring-beans.xsd">
	
	<!-- Root Context: defines shared resources visible to all other web components -->
	
	<bean id="hikariConfig" class="com.zaxxer.hikari.HikariConfig">
		<property name="driverClassName" value="oracle.jdbc.driver.OracleDriver"/>
		<property name="jdbcUrl" value="jdbc:oracle:thin:@192.168.0.187:1521:orcl11"/>
		<property name="username" value="scott"/>
		<property name="password" value="tiger"/>
	</bean>
	
	<bean id="dataSource" class="com.zaxxer.hikari.HikariDataSource" destroy-method="close">
		<constructor-arg ref="hikariConfig"/>	
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
		
</beans>
```

* 
## Final Project

### 개발

* Back-End -&gt; Front\_End - xml -&gt; Java -&gt; 화면
* DML SQL문을 반드시 먼저 테스트 해야한다.
* PL : 복잡한 쿼리문\(조인, 프로시저, 인라인뷰, 서브쿼리, 집계, 통계\)작성을 미리 한다.
* PM : crew관리
* 형상관리 담당자 한명 배정 : git관리, 운영, 테스트

