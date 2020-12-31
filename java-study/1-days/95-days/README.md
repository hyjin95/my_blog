---
description: 2020.12.31 - 95일차
---

# 95 Days -

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org, Toad DBA Suite for Oracle 11.5 , Spring, Android Studio
* 사용 서버 - WAS : Tomcat

## 필기

### jar-jar의 의존관계

* spring-context.jar를 프로젝트에 배포해 ApplicationContext.java 객체주입을 받는다.
* ApplicationContext가 동작하려면 스프링 엔진 spring-core.jar가 있어야만 한다. 서로 의존관계에 있는 jar파일이다.

### jar객체 주입

* Maven - dependency - pom.xml 파일로 관리한다.   JVM 자바 가상머신, Maven-version 등을 관리한다. - Maven Repositoty를 사용한다.
* gradle - dependency
* WEB-INF &gt; lib 에 직접 배포, Build Path에서 라이브러리 등록

### POJO 프레임 워크

* POJO프레임 워크는 req, res객체에 의존적이다.
* HttpServlet을 상속받기 때문에 부모 클래스에게 의존적이게 되는 것이다. doGet, doPost 메서드를 반드시 오버라이드 해야한다.
* 의존적이므로 메서드 구현이나 활용등에서 제약이 발생하기 때문에 상속받는 방식은 결합도가 높다.
* 결합도를 낮추기 위해 나온것이 implements를 활용하는 것이다.
* Spring 프레임 워크는은 인터페이스, 추상클래스 중심의 코딩을 지원한다. 독립성, 재사용성을 높이고 결합도를 낮춘다.
* 여기서 더 나아가 Spring-boot에서는 어노테이션을 제공해 더 높은 자유도를 제공한다.

### get, post 방식

* Post방식으로 전송이 이뤄지는 페이지는 url 쿼리스트링을 통한 단위테스트가 불가능하다.
* 그래서 개발의 초기단계에는 get방식으로 두고 테스트를 완료하면 post로 변경하곤 한다.

## Spring  :  context 환경설정

### url 설정

* 기존 방식으로는 web.xml에 \*.do 를 등록해서 인터셉트했었다. - SimpleUrlHandlerMapping - 위 클래스를 사용하지 않으면 doGet, doPost메서드에서 에러가 발생한다.
* 어노테이션을 활용하면 xml이 없어도 된다. - @RequestMapping\(value="xxx.sp", method="GET\|POST"\)   @GetMapping, PostMapping

### 1. xml : context

```markup
	<!-- Resolves views selected for rendering by @Controllers to .jsp resources in the /WEB-INF/views directory -->
	<beans:bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<beans:property name="prefix" value="/WEB-INF/views/" />
		<beans:property name="suffix" value=".jsp" />
	</beans:bean>
```

### 2. Application.properties : context

```sql
spring.mvc.view.prefix=/WEB-INF/views/
spring.mvc.view.suffix=.jsp
```

### 3. JAVA : context

```java
	public void configureViewResolvers(ViewResolverRegistry registry) {//ViewResolverRegistry는 spring-web에서 제공한다. 디펜던시작성하기
		logger.info("configureViewResolvers 메서드 호출 성공");		
		registry.jsp("/WEB-INF/views/", ".jsp");
	}
```

## Android Atudio

### Andoid의 객체주입

```java
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        //부모클래스 메서드 호출, 메서드 오버라이딩, 부모 생성자 호출, 멤버변수 초기화 등을 하기 위함
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        btn_search = findViewById(R.id.btn_search);
    }
```

* 파라미터로 부모클래스의 상태정보를 받아온다.
* 4번에서 부모의 라이프 사이클 메서드를 호출한다. 메서드 오버라이딩을 사용하고, 부모의 생성자를 호출하거나 멤버변수를 초기화할때 반드시 필요하다.

## Android Atudio: JSON

### 학습목표

* Volley API를 활용해 Thread를 사용하지 않고 JSON형식의 데이터를 처리한다.
* 현대에 공공정보를 JSON의 형태로 제공하는 곳이 많으므로 JSON 데이터의 가공, 처리 능력을 키운다.

### JSON

![](../../../.gitbook/assets/2%20%2881%29.png)

* JSON의 형태가 단일 배열의 형태로 있는 경우도 있지만 위와 같이 배열안에 배열이 또 존재하는 경우도 많이 존재한다.
* 위와 같은 List안에 List가 존재하는 JSON을 출력해보자.

### JSON : dependency

![](../../../.gitbook/assets/1%20%28108%29.png)

* Volley API를 라이브러리에 추가해 Thread없이 통신처리를 허용한다.

![](../../../.gitbook/assets/gson%20%282%29.png)

* GSON 라이브러리를 추가해 JSON형태의 데이터를 처리할 수 있도록 한다.

### 



