# Spring : member

### servlet-context.xml

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

