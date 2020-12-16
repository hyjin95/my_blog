---
description: 2020.12.16 - 85일차
---

# 85 Days -

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5 - Spring - Android Studio
* 사용 서버 - WAS : Tomcat

## Spring4와 Spring Boot

### Spring4와 Spring Boot

* Spring : 외부에서 req, res객체를 받아 메서드를 구성하므로 req, res에 의존적이다.
* Spring-boot : req, res객체가 없더라도 web서비스를 제공해 Spring보다 독립적이다.

### DispatcherServlet과 Controller

* 지정된 url-pattern에 따라 DispatcherServlet이 SimpleUrlHandlerMapping에 등록된 url에 맞는 클래스를 찾아낸다.
* SimpleUrlHandlerMapping클래스가 DispatcherServlet과 각 업무 Controller를 연결하는 역할을 한다.
* 업무 클래스에 대한 구분은 요청된 url-pattern으로 한다.

### url-pattern

* 프로토콜 + 포트번호 + 업무명\(폴더명\) + 페이지이름 + .jsp / .do
* .jsp : HttpServlet, 표준서블릿, sendRedirect
* .do : DispatcherServlet\(Spring\), forward
* 업무명 : SimpleUrlHandlerMapping클래스에서 업무Controller결정\(Spring\)
* 페이지이름 : Properties에서 지정된 메서드 결정\(Spring\)
* Spring에서 클래스등록은 spring-servlet.xml에 작성된다.

### Controller설계

* 하나의 클래스 안에는 n개의 업무가 존재한다.
* 이때 bookInsert업무는 BookInsertController, bookUpdate업무는 BookUpdateController이런식으로 설계를 하면 비효율적인 코드가 탄생할 것이다. 같은 Controller안에서 충분히 나눌 수 있는 업무에 대해 doGet과 같은 메서드 코드가 반복되어야 하는 상황이 발생한다. --기존 POJO방식
* 위와 같은 업무는 BookController안에서 같이 처리하도록 해야 반복 코드를 줄일 수 있다. Spring에서 PropertiesMethodNameResolver클래스를 제공함으로서 이를 가능하게 해준다.

### PropertiesMethodNameResolver

```markup
<bean id="simpleUrlHandlerMapping" class="org.springframework.web.servlet.handler.SimpleUrlHandlerMapping">
		<property name="mappings">
			<props>
				<prop key="/board/boardList.sp3">board-controller</prop>
				<prop key="/board/boardInsert.sp3">board-controller</prop>
				<prop key="/board/boardUpdate.sp3">board-controller</prop>
				<prop key="/board/boardDelete.sp3">board-controller</prop>
			</props>
		</property>
	</bean>   
	
	<bean id="boardController" class="com.mvc3.board.BoardController">
		<property name="methodNameResolver" ref="board-resolver"/>
		<property name="boardLogic" ref="board-logic"/>
	</bean>
	
<bean id="board-resolver" class="org.springframework.web.servlet.mvc.multiaction.PropertiesMethodNameResolver">
		<property name="mappings">
			<props>
				<prop key="/board/boardList.sp3">boardList</prop>
				<prop key="/board/boardInsert.sp3">boardInsert</prop>
				<prop key="/board/boardUpdate.sp3">boardUpdate</prop>
				<prop key="/board/boardDelete.sp3">boardDelete</prop>
			</props>
		</property>
	</bean>
```

* 13번에서 Controller클래스가 ref속성의 board-resolver이름을 가진 &lt;bean&gt;을를 참조하게 한다.

## Eclipse : Spring-boot

### Eclipse : Spring-boot

* Help &gt; Eclipse-Marketplace에서 spring sts를 다운로드 하면 spring-boot프로젝트를 생성할 수 있다.
* File &gt; New &gt; Spring Boot &gt; Spring Starter Project 자바버전을 맞추고 제공 서비스는 Web &gt; Web Service만 체크하고 생성한다.

### Eclipse : Spring-boot-log

```markup
		<!--=========================== log4jdbc로그 추가 ================================-->
		<!-- https://mvnrepository.com/artifact/org.bgee.log4jdbc-log4j2/log4jdbc-log4j2-jdbc4 -->
		<dependency>
		    <groupId>org.bgee.log4jdbc-log4j2</groupId>
		    <artifactId>log4jdbc-log4j2-jdbc4</artifactId>
		    <version>1.16</version>
		</dependency>			
		<!--=========================== log4j-web로그 추가 ================================-->
		<!-- https://mvnrepository.com/artifact/org.apache.logging.log4j/log4j-web -->
		<dependency>
		    <groupId>org.apache.logging.log4j</groupId>
		    <artifactId>log4j-web</artifactId>
		    <version>2.13.3</version>
		</dependency>
```

* MavenRepository : Log4JDBC Log4j2 JDBC4 &gt;&gt; 1.16

  MavenRepository : Apache Log4 Web &gt;&gt; 2.13.3

### 트랜잭션 처리 및 클래스 조립하기

