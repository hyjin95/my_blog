---
description: 2020.12.16 - 85일차
---

# 85 Days - spring-boot : mybatis조립, spring차이, xml, annotation, 트랜잭션, AndroidStudio : Activity, layout

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

```elixir
server.port=8080
#request에 대한 응답페이지 설정 추가
spring.mvc.view.prefix=/WEB-INF/views/
spring.mvc.view.suffix=.jsp
#톰캣서버와 스프링 프로토콜 요청시 한글 설정 추가
server.tomcat.uri-encoding=UTF-8
#spring.http.encoding.charset=UTF-8 내장되어있나?
```

* spring-boot에서는 xml이 아닌 application.properties에 작성한다. 

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

* resources : log4j2.xml + log4jdbc.log4j2.properties + logback.xml

### 트랜잭션 처리

{% page-ref page="eclipse-board-transaction.md" %}

## Eclipse : Spring-boot + MyBatis

### dependency 설정

* MavenRepository
* MyBatis &gt;&gt; 3.5.6
* MyBatis Spring &gt;&gt; 2.0.6
* Gson &gt;&gt; 2.8.6
* HikariCP &gt;&gt; 3.4.5 + application.properties
* Spring Boot Starter JDBC &gt;&gt; 2.3.7 RELEASE

{% page-ref page="mavenrepository-pom.xml.md" %}

### POJO와 Mybatis

```java
Class.forName("오라클 드라이버 클래스 로딩");

Connection con = DriverManager.getConnection(url, scott, tiger);

PreparedStatement pstmt = con.preparedStatement("SELECT 1 FROM dual WHERE id=?);
pstmt.setString(1."test");

ResultSet rs = pstmt.executeQuery();

while(rs.next()){
    String id = rs.getString("id")
}
```

### Spring-boot와 Mybatis

{% page-ref page="eclipse-mybatis.md" %}

## Android Studio

### Activity.java 생성

![](../../../.gitbook/assets/2%20%2867%29.png)

* 패키지 우클릭 &gt; New &gt; Activity &gt; Empty Activity

![](../../../.gitbook/assets/22%20%285%29.png)

* Generate a Layout File : xml문서를 같이 생성할 것인지 여부
* Lancher Activity : manifest에서 메인 화면으로 지정할 것인지

### Activity : Layout xml생성

![](../../../.gitbook/assets/view1.png)

* res &gt; layout우클릭 &gt; New &gt; Layout Resource File

![](../../../.gitbook/assets/view2.png)

* 생성 후 해당 activity xml을 화면으로 하고싶다면 manifest의 MainActivity클래스의 setContentView를 이 xml문서로 지정한다.
* 보통은 Activity java문서를 생성하면 자동으로 xml이 생성된다.

후기 : 저번에 세미프로젝트를 열심히 했던것이 파이널 프로젝트에서도 도움이 되는 것 같다. DB를 해보길 잘했다!

