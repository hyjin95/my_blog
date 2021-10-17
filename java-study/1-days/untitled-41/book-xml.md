# Book : xml

## JAVA

### 코드 : BookController.java

```java
package com.mvc3.board;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.springframework.web.servlet.ModelAndView;
import org.springframework.web.servlet.mvc.Controller;

public class BookController implements Controller{
	
	private BookLogic bookLogic = null;
	
	public void setBookLogic(BookLogic bookLogic) {
		this.bookLogic = bookLogic;
	}

	public ModelAndView handleRequest(HttpServletRequest req, HttpServletResponse res) throws Exception {
		bookLogic.bookList();
		return null;
	}
}

```

* 11번 : 인스턴스 변수를 선언하고,
* 13번 : setter메서드를 작성해 해당 객체가 호출될때에 외부에서 객체주입을 받는다.\
  이때 메서드의 이름은 xml문서에 작성된 property name값과 일치해야만 찾을 수 있다.

### 코드 : BookLogic.java

```java
package com.mvc3.board;

public class BookLogic {

	public void bookList() {
	}
}
```

## XML

### 코드 : spring-servlet.xml

```markup
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:mvc="http://www.springframework.org/schema/mvc"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.3.xsd
		http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context-4.3.xsd
		http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc-4.3.xsd">
		
	<context:component-scan base-package="com.mvc3.board"/>
	
	<bean id="simpleUrlHandlerMapping" class="org.springframework.web.servlet.handler.SimpleUrlHandlerMapping">
		<property name="mappings">
			<props>
				<prop key="/book/bookList.sp3">bookController</prop>
			</props>
		</property>
	</bean>   
	
	<bean id="bookController" class="com.mvc3.board.BookController">
		<property name="bookLogic" ref="book-logic"/>
	</bean>
		
	<!-- viewResolver추가 -->
	<bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<property name="prefix" value="/WEB-INF/board/"></property>
		<property name="suffix" value=".jsp"></property>
	</bean>

</beans>
```

* 어노테이션을 사용하지 않는다면 반드시 xml문서에 클래스가 등록되어 있어야만 찾을 수 있다.

### 코드 : spring-service.xml

```markup
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
	
	<bean id="message-source" class="org.springframework.context.support.ResourceBundleMessageSource">
      <property name="basename" value="messages_ko_KR"/>
    </bean>
    
	<bean id="book-logic" class="com.mvc3.board.BookLogic"/>
    
</beans>
```

* 여기서 10번 코드가 없다면 BookController.java의 18번에서 NullPointerException이 발생한다.
