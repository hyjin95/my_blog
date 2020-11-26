# Spring : member

## servlet-context.xml

### 코드 : servlet-context.xml

```markup
<?xml version="1.0" encoding="UTF-8"?>
<beans 
    xmlns="http://www.springframework.org/schema/beans"
   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   xsi:schemaLocation="http://www.springframework.org/schema/beans https://www.springframework.org/schema/beans/spring-beans.xsd">
   <bean id="default-handler-mapping" class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"/>
   
   <bean id="member-controller" class="com.mycompany.online.MemberController"><!-- Controller등록 -->
   		<property name="methodNameResolver" ref="propertiesPathNameResolver"/><!-- 1에서 이름을 찾아야 함으로 앞에 있어야함 -->
   </bean>
   
   <bean id="simpleUrlHandlerMapping" class="org.springframework.web.servlet.handler.SimpleUrlHandlerMapping">
   		<property name="mappings">
   			<props>
   				<prop key="/test/memberList.nhn">member-controller</prop><!-- 1.controller주입받기 , 메서드 이름과 연결하는 과정 필요 -->
   				<prop key="/test/memberInsert.nhn">member-controller</prop>
   			</props>
   		</property>
   </bean>
   
   <bean id="propertiesPathNameResolver" class="org.springframework.web.servlet.mvc.multiaction.PropertiesMethodNameResolver">
   		<property name="mappings">
   			<props>
   				<prop key="/test/memberList.nhn">memberList</prop><!-- 2.메서드 이름등록, 연결 필요 -->
   				<prop key="/test/memberInsert.nhn">memberInsert</prop>
   			</props>
   		</property>
   </bean>
</beans>
```

* 8-10번은 객체주입을 받거나 외부에서 알아서 호출해야하는 클래스를 등록하는 코드이다. - 첫번째로 필요한 과정 : 클래스 등록
* 15번의 member-controller라는 이름은 memberController클래스의 주소번지이다. - 두번째로 필요한 과정 : 클래스 매핑
* 24번에서는 메서드를 등록하는 과정이 있다. - 두번째로 필요한 과정 : 메서드 등록
* test/memberList.nhn이라는 요청이 들어오면, xml에서 찾아 15번에서 주소번지를 읽는다. 주소번지와 일치하는 id를 가진 8번에서 클래스를 찾고, property에 지정된 ref를 읽는다. ref와 일치하는 id를 가진 21번에서 메서드 이름을 확인한다.
* Console로그를 살펴보면 다음과 같은 구문을 확인할 수 있다. - Mapped URL path \[/online/memberList.nhn\] onto handler 'member-controller' - 해당 path\(url\)이 member\_controller라는 메서드와 연결되어있다는 의미이다.

### 서버 실행시 : Console

![](../../../.gitbook/assets/3%20%2846%29.png)

* 서버를 실행시켜보면 Console에서 xml문서를 읽는 과정을 볼 수 있다.
* 먼저 root-context.xml을 스캔하고, servelet-context.xml을 스캔하는 것을 알 수 있다.

### memberController.java

```java
package com.mycompany.online;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.apache.log4j.Logger;
import org.springframework.web.servlet.ModelAndView;
import org.springframework.web.servlet.mvc.multiaction.MultiActionController;

public class MemberController extends MultiActionController {
	Logger logger = Logger.getLogger(MemberController.class);
	
	public void memberInsert(HttpServletRequest req, HttpServletResponse res)
	throws Exception{
		logger.info("memberInsert 호출성공");
		res.sendRedirect("memberList.jsp");
	}
	
	public ModelAndView memberList(HttpServletRequest request, HttpServletResponse response) {
		logger.info("memberList 호출성공");
		//MemberController가 처리한 결과는 DispatcherServlet에게 전달할 떄 사용하는 클래스
		ModelAndView mav = new ModelAndView();//request scope를 갖는 객체, 자바 forward와 비슷한 역할
		mav.setViewName("a.jsp");
		return mav;
	}
}
```

### memberList.jsp

```markup
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<html>
<head>
	<title>Home</title>
</head>
<body>
<h1>
	Hello world!  
</h1>
</body>
</html>
```

## 결과

### http://localhost:8080/test/memberList.nhn

![](../../../.gitbook/assets/2%20%2859%29.png)

* ModelAndView라는 Spring이 제공해주는 클래스로, 처리 결과를 담아 DispatcherServlet에게 전달할때 사용한다.  - 자바 forward와 비슷하고, request scope를 갖는다.
* 지금은 저 경로에 a.jsp문서가 없어 페이지가 나오지는 않지만, messege문구를 보아 제대로 서블릿 처리가 되었고, a.jsp문서를 찾는단계까지 완료된 것을 볼 수 있다.

### http://localhost:8080/test/memberInsert.nhn

![](../../../.gitbook/assets/1%20%2877%29.png)

* sendRedirect를 사용해 페이지 이동을 했기때문에 주소창은 서블릿 주소인 nhn이 아니라 jsp의 주소가 보여지는 것을 알 수 있다.

