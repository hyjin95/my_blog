---
description: 2020.12.31 - 95일차
---

# 95 Days -

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org, Toad DBA Suite for Oracle 11.5 , Spring, Android Studio
* 사용 서버 - WAS : Tomcat

## Spring  : 환경설정

### jar-jar의 의존관계

* spring-context.jar를 프로젝트에 배포해 ApplicationContext.java 객체주입을 받는다.
* ApplicationContext가 동작하려면 스프링 엔진 spring-core.jar가 있어야만 한다. 서로 의존관계에 있는 jar파일이다.

### jar객체 주입

* Maven - dependency
* gradle - dependency
* WEB-INF &gt; lib 에 직접 배포, Build Path에서 라이브러리 등록

### 1. xml

```markup
	<!-- Resolves views selected for rendering by @Controllers to .jsp resources in the /WEB-INF/views directory -->
	<beans:bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<beans:property name="prefix" value="/WEB-INF/views/" />
		<beans:property name="suffix" value=".jsp" />
	</beans:bean>
```

### 2. Application.properties

```sql
spring.mvc.view.prefix=/WEB-INF/views/
spring.mvc.view.suffix=.jsp
```

### 3. JAVA

```java
	public void configureViewResolvers(ViewResolverRegistry registry) {//ViewResolverRegistry는 spring-web에서 제공한다. 디펜던시작성하기
		logger.info("configureViewResolvers 메서드 호출 성공");		
		registry.jsp("/WEB-INF/views/", ".jsp");
	}
```

