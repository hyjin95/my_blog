# Untitled

## web.xml

```markup
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd" id="WebApp_ID" version="4.0">

<!-- DD파일(Deployment Discriptor) = 배치서술자 -->	
	<servlet>
		<servlet-name>A3Servlet</servlet-name>
		<servlet-class>com.basic.A3</servlet-class>
	</servlet>
	<servlet-mapping>
		<servlet-name>A3Servlet</servlet-name>
		<url-pattern>/EasyUI/a3.do</url-pattern><!-- 주소앞에는 업무명이온다. 업무마다 구분하기위해 -->
	</servlet-mapping>

</web-app>
```

* 11번 - 서블릿 : /EasyUI/a3.do - JSP : dev\_html - webcontent - EasyUI - ...

### JSP 가설세워보기

1. dev\_html - webcontent - EasyUI - xxx.jsp
2. dev\_html - webcontent - EasyUI - datagrid - emp - xxx.jsp
3. dev\_html - webcontent - EasyUI - index.jsp 

위 세가지 가설을 모두 생각해보고 콘솔을 통해 확인해보자

## A

