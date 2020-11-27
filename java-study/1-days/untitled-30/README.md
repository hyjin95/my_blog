---
description: 2020.11.27 73일차
---

# 73 Days -

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5 - Spring
* 사용 서버 - WAS : Tomcat

## 필기

### 순제어

```java
 List<String> insaList = null; //선언
 insaList = new ArrayList<>(); //생성, 활동
 insaList.add("안녕");
 insaList.add("안녕하세요");
 insaList.add("기분좋은 하루 되세요");
 for(String insa:insaList) {
  System.out.println(insa);
 }
 insaList = null; 
```

* 순제어는 개발자가 객체의 생성부터 활동, 종료까지도 관리하는 것이다.
* 2번 : 선언
* 3번 : 생성, 활동
* 10번 : Candidate상태, 종료 - 이때 호출되는 것이 destroy메서드이다. 사라지기 전에 호출된다.

## Spring

### Spring Frameworks Modules

![](../../../.gitbook/assets/spring.png)

* 최근의 개발 트랜드는 개발자는 비즈니스 로직\(Model\)부분에만 관여하는 것이다. - Logic\(POJO\)에만 관여한다.
* 나머지 Controller와 Dao, 조립의 영역은 F/W에게 담당하게 한다. - Spring framwork, 전자정부프레임워크, Dven, AnyFrame
* 이런 F/W들은 DI, IoC를 위한 객체들을 지원한다. - 객체의 라이프사이클을 관리해주기 위함 - 객체=업무는 n개 일 것이다.    이를 관리하는 각각의 Controller가 있고, Controller는 xml에 등록되어야 한다.

### Spring : Controller

* 업무\(객체\)마다 Controller가 필요하다.

## Spring Container\(=엔진, API\) 유형

### 객체를 주입받는 방법 두가지

* spring-core.jar에서 제공해주는 BeanFactory와 ApplicationContext
* spring-core.jar는 Bean을 관리해주는 공장이다. - 공장장 : BeanFactory, ApplicationContext
* Bean을 관리, 필요시 주입하는 역할 - 스프링에게 의존성 주입받는 메서드 : getBean\( \); - 주소번지를 얻는다.

### ApplicationContext

* BeanFactory보다 더 많은 기능을 지원한다.
* 어플리케이션 동작시 Bean이 생성되기를 기다릴 필요가 없어 더 효율적이다.
* Context를 시작시킬때 모든 싱글톤 Bean을 미리 로딩한다.

### 공통점

* Bean을 관리한다.

```markup
<bean id="member-Controller" class="com.xxx.MemberController"/>
```

* Bean의 LifeCycle, 생성과 소멸을 담당한다.

```markup
<bean>
    <property name="listBean"></property>
</bean>
```

* Bean 생성시 필요한 속성을 정의 할 수 있다. - name속성을 정의하는 코드

```java
<Bean id="member-Controller" class="com.xxx.MemberController" 
        init-method="initMethod" destroy-method="destroyMethod"/>
```

* bean의 LifeCycle에 대한 메서드를 호출 할 수 있다. - spring에서도 init\( \) - service\( \) - destory\( \) 메서드를 지원한다.
* init-method : 해당 bean이 초기화된 후 호출되는 메서드
* destory-method : 해당 bean이 소멸되기 전에 호출되는 메서드

### 경로

![ApplicationContext](../../../.gitbook/assets/applicationcontext.png)

![BeanFactory](../../../.gitbook/assets/.png%20%2837%29.png)

* Spring Maven안에 저장된다.

### 의존성 주입이 일어나는 곳

![](../../../.gitbook/assets/di.png)

* ApplicationContext나 BeanFactory가 해준다.

### 코드 : ListMainApp.java

```java
package com.mycompany.online;

import java.util.List;
import org.springframework.beans.factory.BeanFactory;
import org.springframework.beans.factory.xml.XmlBeanFactory;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import org.springframework.core.io.FileSystemResource;
import org.springframework.core.io.Resource;

public class ListMainApp {
	
	List<String> listList = null;
	   public void setListList(List<String> listList) {
	      this.listList = listList;
	   }
	   public static void main(String[] args) {
	      ListMainApp lma = new ListMainApp();
	      
	      //ApplicationContext사용시
	      ApplicationContext context = new ClassPathXmlApplicationContext("com\\\\mycompany\\\\online\\\\insaBean.xml");
	      ListController list2 = (ListController)context.getBean("insaBean");
	      for(String insa:list2.listBean) {
	    	  System.out.println(insa);
	      }
	      
	      //BeanFatory사용시
	      Resource resource = new FileSystemResource("C:\\workspace_sts3\\spring3\\src\\main\\java\\com\\mycompany\\online\\insaBean.xml");
	      BeanFactory factory = new XmlBeanFactory(resource);
	      ListController list = (ListController)factory.getBean("insaBean");
	      System.out.println(list.listBean);
	   }
}
```

### Console 출력

![ApplicationContext](../../../.gitbook/assets/1%20%2879%29.png)

![BeanFactory](../../../.gitbook/assets/2%20%2860%29.png)

## UI : BootStrap

### 공통코드 : bootstrap\_commom.jsp

```markup
<!-- bootstrap 3.4.1 -->
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%
	StringBuilder path = new StringBuilder(request.getContextPath());
	path.append("/");
%>
<link rel="shortcut icon" href="image/favicon.ico">
<link rel="stylesheet" type="text/css" href="<%=path.toString() %>css/bootstrap.min.css">
<script type="text/javascript" src="<%=path.toString() %>js/jquery.min.js"></script>
<script type="text/javascript" src="<%=path.toString() %>js/bootstrap.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery-cookie/1.4.1/jquery.cookie.min.js"></script>
```

