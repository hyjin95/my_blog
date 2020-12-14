---
description: 2020.12.14 - 83일차
---

# 83 Days -



## 필기

### JSP와 인스턴스화

* &lt;jsp:useBean id="tv" class="com.TV scope=" "/&gt; 스코프가 없어 클래스를 유지할 수 없다.
* JSP에서는 인스턴스화하지 않는다.

## Spring

### @RequestMapping

* 3.0 이전 Spring - 모두 xml을 이용해 처리했다. - SimpleUrlHandlerMapping, PropertiesMethodNameResolver 이 두 클래스가 있어야 url패턴에 대응하는 메서드 이름을 찾을 수 있었다.
* 5.0 이전 Spring - 주로 자바를 이용해 처리하는 방식으로 변화했다.\(초보자들을 위한 배려\) - 메서드 선언 앞에 어노테이션 @RequestMapping을 사용할 수 있게 되었다. - @GetMapping, @PostMapping

### ViewResolver

```markup
	<!-- viewResolver추가 -->
	<bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<property name="prefix" value="/WEB-INF/board/"></property>
		<property name="suffix" value=".jsp"></property>
	</bean>
```

* 응답 페이지에 대한 url을 추가하는 클래스
* xml에 작성한다.

## Spring4\(Spring boot\)

### New Project

### Maven 방식

### boot 방식

* xml이 아닌 properties에서 작성한다.

### pom.xml

```markup
	<dependencies>
	<!--=========================== spring-boot 시 ================================-->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web-services</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.session</groupId>
			<artifactId>spring-session-core</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
			<exclusions>
				<exclusion>
					<groupId>org.junit.vintage</groupId>
					<artifactId>junit-vintage-engine</artifactId>
				</exclusion>
			</exclusions>
		</dependency>
	<!--=========================== spring-boot 끝 ================================-->
	
	<!--=========================== 톰캣 의존성주입 시작[jsp문서 인식하게하기] ================================-->
		<dependency>
			<groupId>org.apache.tomcat.embed</groupId>
			<artifactId>tomcat-embed-jasper</artifactId>
		</dependency>
	<!--=========================== 톰캣 의존성주입 끝  ================================-->
	</dependencies>
```

