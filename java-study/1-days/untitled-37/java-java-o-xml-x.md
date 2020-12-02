# Java : Java - @ \(O\), xml \(X\)

## com.di

### 코드 : AppContext.java

```java
package com.di;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import com.spring.mvc1.DeptController;

@Configuration//import할 수 있다. java:java에서만 사용할 수 있다.ㅅ 스프링에서 객체를 주입받을 수 있게 해준다.
public class AppContext {
	
	//<bean id="deptController" class="com.do.DeptController/>이 xml을 자바에서 어노테이션을 활용하면 할 수 있다.
	@Bean//Spring이 관리할 수 있는 Bean으로 만들어준다.
	public DeptController deptController() {//메서드 이름이 다르면 ApplicationContext, BeanFactory가 찾을 수 없다.
		
		return new DeptController();//new를 spring이 해주므로 일반적으로 개발자가 직접하는 거랑은 차이가 있다.
	}
	
	@Bean
	public DeptController deptLogic() {
		
		return new DeptController();//new를 spring이 해주므로 일반적으로 개발자가 직접하는 거랑은 차이가 있다.
	}
	
	@Bean
	public DeptController deptDao() {
		
		return new DeptController();//new를 spring이 해주므로 일반적으로 개발자가 직접하는 거랑은 차이가 있다.
	}
}
```

### 코드 : MainForDept.java

```java
package com.di;

import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

import com.spring.mvc1.DeptController;

public class MainForDept {

	public static void main(String[] args) {
		
		//어노테이션으로 등록된 메서드를 사용할 수 있다.
		ApplicationContext context = new AnnotationConfigApplicationContext(AppContext.class);
		
		DeptController deptController = context.getBean("deptController", DeptController.class);
		System.out.println(deptController.deptList());

	}
}
```

## com.spring.mvc1

### 코드 : DeptController.java

```java
package com.spring.mvc1;

import java.util.List;
import java.util.Map;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.apache.log4j.Logger;
import org.springframework.stereotype.Controller;
import org.springframework.web.servlet.ModelAndView;
import org.springframework.web.servlet.mvc.multiaction.MultiActionController;

@Controller
public class DeptController {
	Logger logger = Logger.getLogger(DeptController.class);
	
	//이때 변수명은 property의 name에 들어갈 값과 일치해야한다.
	private DeptLogic deptLogic = null;
	
	public void setDeptLogic(DeptLogic deptLogic) {
		this.deptLogic = deptLogic;
	}

	public ModelAndView deptList() {
		logger.info("deptList 호출성공");
		List<Map<String,Object>> deptList = null;
		//deptList = deptLogic.deptList();
		ModelAndView mav = new ModelAndView();
		mav.addObject("deptList", deptList);
		mav.setViewName("dept/deptList");//WEB-INF/views/[[dept/deptList]].jsp
		return mav;
	}
}
```

* xml을 활용하지 않을때에는 어노테이션@을 사용해 주입받을 수 있다.
* @Controller를 활용한 Controller클래스
* 28번은 Dao-DB를 구현하지 않아 null이발생하므로 일단 주석처리하고 26번이 찍히는 지 확인한다.

## Console

### 결과 : Console

![](../../../.gitbook/assets/555%20%281%29.png)

