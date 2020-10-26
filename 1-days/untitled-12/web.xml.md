# web.xml

```markup
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd" id="WebApp_ID" version="4.0">
<!-- 
sever.xml은 톰캣서버가 기동할떄 디폴트, 기본으로 읽게되는데
xml의 규칙 내에서 포트번호를 결정하고 프로젝트를 배치한다. 
-->
<!-- log4j 환경파일 등록하기 -->
	<context-param>
		<param-name>log4jConfigLocation</param-name><!-- 객체주입 -->
		<param-value>/WEB-INF/classes/log4j.properties</param-value><!-- 톰캣서버가 읽을 수 있게 한다. -->
	</context-param>
<!-- DD파일(Deployment Discriptor) = 배치서술자 -->
	<servlet>
		<servlet-name>testMgr</servlet-name>
		<servlet-class>web.mvc.TestController</servlet-class>
	</servlet>
	<servlet-mapping>
		<servlet-name>testMgr</servlet-name>
		<url-pattern>/test/test.do</url-pattern><!-- url -->
		<!-- 웹서비스를 할때에 자바를 인스턴스화할 수 없으므로 url로 접근한다. 이를 위해 xml dd파일에 url을 생성해야한다. -->
	</servlet-mapping>
</web-app>
```

