---
description: 2020.12.09 - 81일차
---

# 81 Day - Spring : 특징, jar:jar, AndroidStudio : 새 프로젝트, 이클립스 단축키 설정, mainfest, Spring4 환경설정

### 사용 프로그램

* 사용언어 : JAVA(JDK)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5 - Spring
* 사용 서버 - WAS : Tomcat

## 필기 : 기술면접

### Spring F/W 장점

* 빠른 구현 시간\
  \- 골격드를 제공해줘 반복되는 코드를 줄여준다.\
  \- 골격코드 : 인터페이스, 추상클래스, 구현체클래스\
  \- 개발자는 비즈니스로직(Model계층:Logic+Dao)만 구현하면 된다.
* 쉬운관리\
  \- 골격코드를 제공해줌으로서 통일성, 일관성이 보장된다.
* 개발자들의 역량 일관성(평균)\
  \- 마찬가지로 골격코드로 인해 역량이 다른 개발자 이더라도 같은 수준의 기능을 구현 하기 쉽게 한다.
* 안전성, 재사용성\
  \- 검증된 아키텍쳐를 사용하므로 안전하고 재사용성이 높다.

### Spring F/W 특징

* 경량화\
  \- 인터페이스와 추상클래스를 줄여 가볍다.
* ioC\
  \- 제어역전\
  \- 객체를 외부에서 주입받음으로서 외부에서 라이프사이클을 관리한다.\
    A a = null; -> setA( )
* AOP\
  \- 트랜잭션처리
* Container\
  \- spring-core.jar : BeanFactory, ApplicationContext\
  \- BeanFactory와 ApplicationContext가 bean으로 등록된 클래스들을 관리한다.
* DI\
  \- 의존성주입\
  \- 대신 주입해준다.
* 배치서술자 파일에 서블릿을 등록한다.\
  \- Java : HttpServlet상속-Servlet, Object상속-Java
* xml 분할\
  \- spring-servlet : 컨트롤러\
    spring-service : 로직\
    spring-data : xml-xml의 객체주입이 일어나는 곳, DB연결에 대한 정보

### Java Servlet

* HttpServlet상속-Servlet\
  Object상속-Java
* HttpServlet을 상속받으면 doGet, doPost메서드 에서는 request, response를 사용할 수 있다.
* url을 통한 접근을 처리할 수 있다.\
  단, 보안에 취약해 jsp파일에 외부에서 접근할 수 있다.
* getRequestContext : server.xml에 있는 context에 직접 접근할 수 있게 해준다.\
  서버가 내부 WEB-INF밑에 존재하는 jsp파일에 접근 할 수 있게 해준다.

## jar : jar

### jar : jar

* jar의 안에는 클래스, 인터페이스, 추상클래스가 있다.
* spring-beans.jar는 spring-core.jar가 반드시 존재해야 하는 등 jar안에서도 의존관계가 존재한다.\
  컴파일 문제를 피하기 위해서는 의존관계에 있는 jar파일이 배포되어 있어야 한다.
* 컴파일 에러가 발생한다. -> 의존관계가 존재한다. -> jar파일로 해결한다.

## Spring 1-2

### 새 프로젝트 생성

{% content-ref url="spring4-1-2.md" %}
[spring4-1-2.md](spring4-1-2.md)
{% endcontent-ref %}

### spring-servlet.xml

```markup
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:mvc="http://www.springframework.org/schema/mvc"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.3.xsd
		http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context-4.3.xsd
		http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc-4.3.xsd">
		
		<context:component-scan base-package="com.mvc2"/>

</beans>
```

* 새로 생성한 java src 패키지 지정하기

{% content-ref url="spring-spring-sevlet.xml.md" %}
[spring-spring-sevlet.xml.md](spring-spring-sevlet.xml.md)
{% endcontent-ref %}

### MavenRepository

* 수동 :  jar다운로드 > jar파일 lib폴더에 배포
* 핵심 엔진 : spring-core 4.3.29\
  \- log : Apache-Commons-Logging 1.2
* web : spring-web 4.3.29\
  \- aop : spring-aop 4.3.29\
  \- beans : spring-beans 4.3.29\
  \- context : spring-context 4.3.29
* jdbc : spring-jdbc 4.3.29
* Expression : spring-expression-language(SpEL) 4.3.29
* MVC : spring-webmvc 4.3.29
* driverClass : ojdbc6.jar
* myBatis : mybatis 3.4.6\
  myBatis-spring : mybatis-spring 1.3.3
* Gson : gson 2.8.6
* 프로젝트 우클릭 > Build Path > Configure Build Parh > Libraries 에서 lib폴더에 배포한 jar파일 add

### Dependency - Maven

![mybatis](../../../.gitbook/assets/mybatis.png)

![mybatis-spring](../../../.gitbook/assets/mybatis_spring.png)

![gson](../../../.gitbook/assets/gson.png)

* 이전에 \<dependency>태그로 maven저장소를 활용해 추가했던 mybatis, mybatis-spring, gson은 사용자 문서 안에 존재하므로 가져다 추가하자.
* mybatis-spring.jar는 sqlSessionFactory, sqlSessionTemplate를 제공해준다.

## Android Studio

### Android Studio

![](../../../.gitbook/assets/kakaotalk\_20201209\_144559165.jpg)

* 디바이스가 앱을 실행하면 액티비티 객체가 만들어진다.\
  이때 이 액티비티는 화면을 지원받아야하므로 레이아웃을 지정한다.
* 액티비티는 화면에 액티비티를 표시하라고 안드로이드에게 지시한다.
* 사용자는 디바이스에 나타난 레이아웃과 상호작용 하게 된다.\
  \- Event, 소통
* 액티비티는 어플리케이션 코드를 실행해서 사용자의 동작에 응답한다.
* 사용자는 그 결과를 디바이스로 확인할 수 있게 된다.

### 프로젝트명 : Hello69Ver1.apk

* 안드로이드 프로젝트 생성
* 화면 디자인 및 편집 : \*.xml
* Java 코드 작성 및 수정 : \*.java(jdk8.0 : 부분지원)
* 프로젝트 실행 및 결과 확인

### mainfest

* 앱에 대한 벙보를 적재하는 곳
* AndroidMainfest.xml\
  \- 앱에 대한 이름\
  \- 앱에 대한 이미지 정보\
  \- 앱에 대한 메인 액티비티 이름 등록

### 이클립스 단축키 사용하기

![](<../../../.gitbook/assets/.png (46).png>)

### 새 프로젝트 생성

{% content-ref url="android-studio.md" %}
[android-studio.md](android-studio.md)
{% endcontent-ref %}

후기 : 프로젝트에도 앱을 구현해야하기 때문에 AndroidStudio를 열심히 해야한다!
