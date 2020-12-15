# Board : Annotation

## JAVA

### 코드 : BoardController.java

```java
package com.mvc3.board;

import java.util.List;
import java.util.Map;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;

@Controller
@RequestMapping("/board/*")
public class BoardController {
	
	@Autowired
	public BoardLogic bLogic = null;

	@RequestMapping("/boardList.sp3")
	public String boardList(@RequestParam Map<String,Object> pMap) {
		logger.info("boardList 호출 성공 : "+pMap);
		List<Map<String,Object>> bList = null;
		bList = bLogic.boardList(pMap);
		return "foward:list.jsp";
	}
}
```

* @Controller  서블릿을 직접 상속 받는 대신 어노테이션을 사용해 결합도를 낮춘다. 메서드의 파라미터로 req, res가 없어도 다른 클래스의 메서드가 호출된다. req, res객체에 의존하지 않고도 웹 서비스를 제공 할 수 있다.
* @RequestMapping\( \) 이 어노테이션이 url-pattern과 매핑해주는 역할을 한다. /board/가 들어가는 모든 .sp3 url요청은 이 BoardController클래스가 인터셉트한다.
* @Autowired 기존방식의 setter메서드를 사용하지 않고도 객체를 주입받을 수 있게 해주는 어노테이션이다.
* @RequestParam 기존에 req.getParameter로 받아오던 값을 파라미터로 받아올 수 있게 해주는 어노테이션이다. url과 같이 전송되는 data는 boardList의 파라미터인 pMap에 모두 담긴다.
* String 타입 메서드 기존의 void, ModelAndView가 아닌 String타입의 메서드를 활용한다. 

### 코드 : BoardLogic.java

```java
package com.mvc3.board;

import java.util.List;
import java.util.Map;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class BoardLogic {
	
	@Autowired
	public BoardDao boardDao = null;
	
	public List<Map<String,Object>> boardList(Map<String, Object> pMap) {
		logger.info("Logic - boardList 호출 성공 : "+pMap);
		List<Map<String,Object>> bList = null;
		bList = boardDao.boardList(pMap);
		return bList;
	}
}
```

### 코드 : BoardDao.java

```java
import java.util.ArrayList;
import java.util.List;
import java.util.Map;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class BoardDao {
	
	public List<Map<String, Object>> boardList(Map<String, Object> pMap) {
		List<Map<String,Object>> bList = new ArrayList();
		return bList;
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
	
	<!-- viewResolver추가 -->
	<bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<property name="prefix" value="/WEB-INF/board/"></property>
		<property name="suffix" value=".jsp"></property>
	</bean>
</beans>
```

* 해당 패키지를 등록하는 것 외에 다른 코드 작성은 필요없다.

