# MavenRepository : pom.xml

### demo/pom.xml

```text
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

