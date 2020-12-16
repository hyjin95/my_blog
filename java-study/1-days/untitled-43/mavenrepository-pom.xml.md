# MavenRepository : pom.xml

### demo/pom.xml

```markup
	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
		<!--=========================== JDBC 추가 관련 ================================-->
		<!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-jdbc -->
		<dependency>
		    <groupId>org.springframework.boot</groupId>
		    <artifactId>spring-boot-starter-jdbc</artifactId>
		    <version>2.3.7.RELEASE</version>
		</dependency>
		<!--=========================== 톰캣 의존성주입 [jsp문서 인식하게하기] ================================-->
		<dependency>
			<groupId>org.apache.tomcat.embed</groupId>
			<artifactId>tomcat-embed-jasper</artifactId>
		</dependency>
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
		<!--=========================== mybatis 레이어 추가관련 ================================-->
		<!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
		<dependency>
		    <groupId>org.mybatis</groupId>
		    <artifactId>mybatis</artifactId>
		    <version>3.5.6</version>
		</dependency>
		<!--=========================== mybatis와 spring 연계 레이어 추가관련 ================================-->
		<!-- https://mvnrepository.com/artifact/org.mybatis/mybatis-spring -->
		<dependency>
		    <groupId>org.mybatis</groupId>
		    <artifactId>mybatis-spring</artifactId>
		    <version>2.0.6</version>
		</dependency>
		<!--=========================== JSON 추가 관련 ================================-->
		<!-- https://mvnrepository.com/artifact/com.google.code.gson/gson -->
		<dependency>
		    <groupId>com.google.code.gson</groupId>
		    <artifactId>gson</artifactId>
		    <version>2.8.6</version>
		</dependency>
		<!--=========================== HikariCP 추가 관련 ================================-->
		<!-- https://mvnrepository.com/artifact/com.zaxxer/HikariCP -->
		<dependency>
		    <groupId>com.zaxxer</groupId>
		    <artifactId>HikariCP</artifactId>
		    <version>3.4.5</version>
		</dependency>
		
	</dependencies>
```

### application.properties

```elixir
server.port=8080
#request에 대한 응답페이지 설정 추가
spring.mvc.view.prefix=/WEB-INF/views/
spring.mvc.view.suffix=.jsp
#톰캣서버와 스프링 프로토콜 요청시 한글 설정 추가
server.tomcat.uri-encoding=UTF-8
#spring.http.encoding.charset=UTF-8 내장되어있나?
#HikariCP 커넥션 풀 설정 추가
spring.datasource.hikari.driver-class-name=oracle.jdbc.driver.OracleDriver
spring.datasource.hikari.jdbcUrl=jdbc:oracle:thin:@127.0.0.1:1521:orcl11
spring.datasource.hikari.username=scott
spring.datasource.hikari.password=tiger
spring.datasource.hikari.connection-timeout=20000
spring.datasource.hikari.minimum-Idle=5
spring.datasource.hikari.maximum-pool-size=12
spring.datasource.hikari.ldle-timeout=300000
spring.datasource.hikari.max-lifetime=1200000
spring.datasource.hikari.auto-commit=true
```

