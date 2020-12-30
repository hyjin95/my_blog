---
description: 2020.12.30 - 94일차
---

# 94 Days -

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org, Toad DBA Suite for Oracle 11.5 , Spring, Android Studio
* 사용 서버 - WAS : Tomcat

## Spring

### Spring 환경설정 방법

1. xml기반 환경설정
2. xml대신 application.properties 설정
3. java로 환경 설정

### ViewResolver 

```markup
	<!-- viewResolver추가 -->
	<bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<property name="prefix" value="/WEB-INF/views/"></property>
		<property name="suffix" value=".jsp"></property>
	</bean>
```

* 응답페이지의 url-pattern을 정의한다.
* xml 설정파일 spring-servlet.xml에 작성된다. ViewResolver중에서 jsp를 View로 사용할 때에는 InternalResourceViewResolver클래스를 사용한다. spring에서 제공하는 클래스를 xml에 작성해 사용하는 것이지만 JAVA에서도 import를 통해 구현가능
* Controller계층에서 응답시 사용하게된다.
* 클라이언트의 직접 접근을 차단해 보안성을 높이는 코드로 jsp사용시 항상 필요하다.
* ViewResolver로 연결되는 페이지들은 WEB-INF 하위에 위치한 페이지에 연결된다.

## Spring : 첨부파일

### Spring : Controller

```java
	public void boardInsert(HttpServletRequest req, HttpServletResponse res)
			throws Exception{
		logger.info("controller - boardInsert호출성공");
		int result = 0;
		Map<String,Object> pmap = new HashMap<>();
		HashMapBinder hmb = new HashMapBinder(req);
		//첨부파일을 처리할 때에는 반드시 post방식이여야 한다.
		//주의사항 : cos.jar에서 제공하는 MultipartRequest를 사용해야 값 요청 가능
		hmb.multiBind(pmap);
		result = boardLogic.boardInsert(pmap);//첨부파일 여부는 logic에서 처리한다.
		if(result==1) {
			res.sendRedirect("/board/boardInsertOk.jsp");//insert성공 후 목록 갱신
		}else {
			res.sendRedirect("/board/boardInsertFail.sp");
		}
	}
```

* HttpServlet을 주입받아 사용한다.
* POJO로 작성한 HashMapBinder클래스를 생성, 요청객체를 넘겨 사용자 입력값, 첨부파일을 받아온다.
* 기존 request객체로는 파일타입을 처리할 수 없어 cos.jar를 추가해 MultipartRequest객체를 사용한다.
* 응답도 HttpServlet을 주입받아 response객체를 사용해 응답한다.

### Spring-boot : Controller

```java
  //첨부파일은 어떻게 받지?? RequestParam을 하나 더 설정한다. 어노테이션뒤에 ( )안에 속성을 설정할 수 있다.
  //@RequestMapping("/boardInsert.sp3")
	@PostMapping("/boardInsert.sp3")
	public String boardInsert(@RequestParam Map<String,Object> pMap
							, @RequestParam(value="bs_file", required=false) MultipartFile bs_file) {//required=false, 첨부파일은 없을 수 있다.
		
		logger.info("controller - boardInsert호출성공");
		int result = 0;
		result = boardLogic.boardInsert(pmap);//첨부파일 여부는 logic에서 처리한다.
		if(result==1) {
			return "redirect:boardInsertOk.jsp";
		}else {
			return "redirect:boardInsertFail.sp";//Insert이기때문에 forward일 필요 없다.
		}
	}
```

* 첨부파일이 포함될 수 있으므로 Post방식으로 요청이 들어올것이므로 @PostMapping어노테이션을 사용했다.
* RequestParam 1 : 사용자 입력값을 받아올 파라미터 Map, HashMapBinder클래스 역할을 수행한다.
* RequestParam 2 : 첨부파일을 받아올 파라미터 MultipartFile - 첨부파일은 필수조건이 아니기 떄문에 required=falsle속성을 추가해 없더라도 지나가게 처리한다.
* 타입이 String이므로 return이 필요하고 응답처리는 response객체를 주입받아 사용하지 않는다.

## Spring : JAVA 환경설정

### ViewResolver : xml 

```markup
<!-- viewResolver추가 -->
	<bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<property name="prefix" value="/WEB-INF/views/"></property>
		<property name="suffix" value=".jsp"></property>
	</bean>
```

* 기존의 spring-servlet.xml에서 작성한 viewResolver 환경설정

### ViewResolver : JAVA

```java
package config;

import java.util.List;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.EnableWebMvc;
import org.springframework.web.servlet.config.annotation.ViewResolverRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
@EnableWebMvc
public class MvcConfig implements WebMvcConfigurer {
	Logger logger = LoggerFactory.getLogger(MvcConfig.class);
	
	public void configureViewResolvers(ViewResolverRegistry registry) {//ViewResolverRegistry는 spring-web에서 제공한다. 디펜던시작성하기
		logger.info("configureViewResolvers 메서드 호출 성공");
		
		registry.jsp("/WEB-INF/views/", ".jsp");
	}
}
```

* 자바에서 어노테이션을 활용해 작성한 환결설정 클래스
* 20번 registry.jsp\( \) - 첫번째 파라미터 : "perfix"\(접두어\) - 두번째 파라미터 : "suffix"\(접미어\)

```markup
    <dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-web</artifactId>
			<version>${org.springframework-version}</version>
		</dependency>
```

* pom.xml에 spring-web dependency추가
* 자바버전 1.6 -&gt; 1.8
* org.springframework-version 3.1.1 -&gt; 5.1.16

![](../../.gitbook/assets/1%20%28107%29.png)

* 위 pom.xml의 수정을 마쳐야 spring-web에서 제공하는 ViewResolverResgistry을 사용할 수 있게된다.
* registry.jsp를 지원하는 것을 볼 수 있다.

### Package\(component\) Scan : xml

```markup
<!-- spring_context.jar에서 제공하는 속성, 스캔할 베이스 패키지를 지정한다. -->
	<context:component-scan base-package="com.di"/>
	<context:component-scan base-package="com.board"/>
```

* 기존에 spring-servlet.xml에서 작성했던 component-scan 설정 속성
* 여러 패키지를 베이스 패키지로 스캔하도록 지정할 수 있다.

### Package\(component\) Scan : JAVA

```java
package config;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;

@Configuration //환경설정 클래스 선언
@ComponentScan(basePackages= {"config"})//자바로 사용할 패키지 컴포넌트 스캔 지정하기.
@ComponentScan(basePackages= {"web.android"})//스캔할 베이스 패키지 추가
public class RootConfig {

}
```

* @Configuration 어노테이션으로 작성한 자바 환경설정파일 클래스
* @ComponentScan 어노테이션으로 component-scan설정을 할 수 있다. xml문서와 똑같이 여러 패키지를 베이스 패키지로 스캔하도록 지정할 수 있다.

### web.xml 수정

```markup
<!-- Processes application requests -->
	<servlet>
		<servlet-name>appServlet</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
		<init-param>
			<param-name>contextClass</param-name>
			<param-value>org.springframework.web.context.support.AnnotationConfigWebApplicationContext</param-value>
		</init-param> 
		<init-param>
			<param-name>contextConfigLocation</param-name>
			<param-value>
				config.RootConfig
				config.MvcConfig
			</param-value>
		</init-param> 
		<load-on-startup>1</load-on-startup>
	</servlet>
```

* 환경설정 어노테이션을 읽을 수 있도록 5-8번에서 클래스를 읽게한다.
* 9-15번에서 @Configuration 어노테이션으로 작성한 환경설정 자바 파일을 등록해 읽게 한다.

```markup
<!-- Processes application requests -->
	<servlet>
		<servlet-name>appServlet</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
		<init-param>
			<param-name>contextConfigLocation</param-name>
			<param-value>/WEB-INF/spring/appServlet/servlet-context.xml</param-value>
		</init-param>
		<load-on-startup>1</load-on-startup>
	</servlet>
```

* 위는 자동으로 생성되었던 기존코드
* 자동생성된 servlet-context.xml을 환경설정파일로 읽고 있다.

![](../../.gitbook/assets/.png%20%2851%29.png)

* servlet-context.xml
* 기존에 자동으로 생성되는 ViewResolver를 정의한 xml문서를 web.xml에서 지정 해제 하고 작성한 java문서를 매칭시켜야 한다.

### 환경설정 Test 

* Java파일이 제대로 설정, 등록이 완료되었는지 확인해보자

![](../../.gitbook/assets/1%20%28106%29.png)

* TestController를 생성하고 WEB-INF &gt; views에 test.jsp를 만들었다.

```java
package web.android;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

@Controller
public class TestController {
	Logger logger = LoggerFactory.getLogger(TestController.class);
	
	@RequestMapping(value="test.ko", method=RequestMethod.GET)
	public String test() {
		logger.info("test 메서드 호출 성공");
		return "test";
		//return "redirect:test";//404,ViewResolver를 사용하지 않는 응답이다. .jsp를 붙여야 됨
		//return "forward:test";//404,.jsp를 붙여도 404가 발생한다. 이거는 webapp에 접근하니까 webapp에 jsp가 존재해야한다.
	}
}
```

* TestController 코드
* 어노테이션 @Controller를 작성해 컨트롤러로 등록한다.
* @RequestMapping 어노테이션을 메서드에 작성해 url-pattern을 등록한다. - value속성으로 인터셉트할 url-pattern지정 - method속성으로 GET방식을 받아오게 지정

![](../../.gitbook/assets/2%20%2880%29.png)

* url : /step3/test.jo로 요청하면 test.jsp가 보여지는것을 확인할 수 있다.

