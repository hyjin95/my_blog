---
description: 2020.12.15 - 84일차
---

# 84 Days -

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5 - Spring - Android Studio
* 사용 서버 - WAS : Tomcat

## Spring : MVC 

### request, response

* void\(Lv1\) -&gt; ModelAndView\(Lv2\) -&gt; String\(Lv3\)
* req, res를 사용해야한다는 것은 상속을 받아야 하므로 의존적인 구성이 된다는 것이다.
* void타입과 ModelAndView타입을 활용하는 메서드를 갖는 클래스는 req, res를 상속받아야 한다.
* 이를 벗어나기 위해 String타입을 사용하게 된다.

### String

* string 타입의 메서드를 활용하면서 req와 res를 주입받지 않아도 되게 되면서 페이지간의 관계가 더 자유로워 지게 되었다.
* req, res를 받지 않으므로 forward와 sendRedirect, getAttirbute, getParamter, setAttribute를 사용할 수 없게 되는 대신, 어노테이션을 활용해 이와 같은 역할을 똑같이 수행할 수 있다.
* 기존의 ModelAndView에 담던 페이지 이동은 리턴값을 이용한다. - return "redirect:xxx.jsp";

### 추상클래스와 인터페이스

* 인터페이스를 활용하는 경우, 타입과 파라미터가 고정된 메서드를 오버라이드 해야만한다.
* 하지만 추상클래스를 활용하는 것보다는 재사용성이 높다.
* 인터페이스는 자체적으로 인스턴스화 될 수 없기때문에 구현체 클래스를 활용하고, 이 다형성으로 인해 같은 인터페이스 이더라도 다른 결과를 출력할 수 있기 때문이다.

## Spring : Annotation

### 어노테이션을 사용하지 않는 경우

* spring-servlet.xml : 컨트롤계층 + ViewResolver + Resource\(상수 : 이미지, 애니메이션, 색상, ....\) spring-service.xml spring-data.xml mybatis-config.xml
* xml문서에 매번 등록해야 하고, java와 여러 xml문서를을 동기화해야하므로 복잡하다.
* 동기화 - DispatcherServlet과 업무별 Controller - XXXController와 XXXLogic : java와 java - XXXLogic과 SqlXXXDao : java와 java - java와 java이지만 일반 인스턴스화가 아닌 xml을 통한 외부 주입을 받기 위한 코드가 필요하다.

### 어노테이션을 사용하지 않는 경우 -2

* ViewResolver - 응답할 view 페이지에 대한 url정의 - 접두어 : "WEB-INF/views/"   접미어 : ".jsp" - 작성시 "/"와 응답페이지를 호출하는 클래스에서의 페이지 이름에 ".jsp"가 붙는지 주의 한다.
* ViewName - Lv2   ModelAndView mav = new ModelAndView\( \);    mav.setViewName\("xxxList"\) 

### 어노테이션을 사용하는 경우

* 컨트롤 계층 : @Controller 모델     계층 : @Service
* @Autowired 어노테이션을 인스턴스 변수에 붙이기만 하면 된다.

### url-pattern : @RequestMapping , DI : @Autowired

```java
package com.mvc3.board;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
@RequestMapping("/board/*")
public class BoardController {	
	
	@Autowired
	public BoardLogic boardLogic = null;
	
	@RequestMapping("/boardList.sp3")
	public String boardList(Map<String,Object> pMap) {
		logger.info("boardList 호출 성공 : "+pMap);

		return "foward:list.jsp";
	}
}
```

* 어노테이션과 String을 활용한다면 기존의 url-pattern은 어떻게 작성 할까?
* 역시 어노테이션을 활용한다. : @RequestMapping
* Controller클래스는 @Controller 어노테이션을 작성하고, 그 밑에 url을 작성한다. @RequestMapping\(/member/\*\) or \(/order/\*\)
* 그리고 Controller안에서 구현하는 메서드에 업무내용 url을 작성해 분류한다. @RequestMapping\(/memberList.do\) or \(/memberInsert.do\)
* 의존성 주입은 xml작성 대신에 멤버변수에 어노테이션 @Autowired를 붙인다.

