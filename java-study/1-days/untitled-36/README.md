---
description: 2020.11.30 - 74일차
---

# 74 Days -

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5 - Spring
* 사용 서버 - WAS : Tomcat

## 필기

### MVC패턴

*  Controll계층 - 추상클래스나 인터페이스 혹은 구현체 클래스를 제공받아 처리한다. - AbstracController\(A\), MultiActionController\(구현체\), Controller\(I\)
* Model 계층 - 개발자가 집중해야하는 부분
* View 계층 - UI/UX솔루션 역할
* 요청을 jsp가 받으면 모델1 : 태그와 공존해야 하므로 내장객체를 제공한다. 요청을 Servlet이 받으면 모델2 : 직접 선언해야한다.
* 이 MVC패턴은 request와 response에 의존적이다. 재사용성, 이식성이 떨어져 단위테스트, 통합테스트에 어려움이 있다. 독립적이지 않기때문에 결합도를 낮추는 코드를 작성해야한다. - 상속이아닌, 인터페이스와 추상클래스를 활용한다.

### Model계층

* DAO패턴은 MODEL계층의 한 부분이다.
* DAO가 있으므로 트랜잭션 처리가 가능하다.
* 재사용성을 높인다. - 업무와 업무가 연결되는 부분을 재사용해 처리 할 수 있게 해준다.

### 개발 방법론

* 개발자는 로직부분에만 집중하고 코딩을 전개한다.
* 이렇게 하기 위해 MVC패턴이나 Spring을 활용한다.
* 넥사크로, 부트스트랩, EasyUI같은 UI솔루션이나 API가 제공되고 있다.
* DB연동시 반복되는 코드를 줄이기 위해 MyBatis를 사용한다.
* git과 같은 형상관리 툴을 사용하면 재택근무가 가능하다.

## 팀프로젝트 준비

### 페이퍼 작업

* 역할 분담
* 계획\(시간을 효율적으로 활용\)
* 표준 - 약속
* 팀프로젝트가 100%라면 - 50% : 계획, 설계, 분석\(회사문서, 보고서, 자료수집, 자료분석, 업무 정의, 프로세스이해\) - 20% : 코딩 - 10% : 테스트

### 역할

* ProjectManager : 사람관리
* ProjectLeader : 설계, 조립, 테스트시나리오, 배포, 이행 - 기술지원, 표준제시, 가이드
* Crew : 처리

## Session

### Session과 Cookie

* session - 서버의 cash메모리에 클라이언트 상태를 저장한다.
* cookie - 클라이언트 local에 text로 저장된다. - cookie에 가장 먼저 저장되는 정보 : Session ID

### Session과 request

```java
HttpSession session = request.getSession();
```

* 내장객체 request로 session을 생성한다.

### Session과 web.xml

```markup
	<!-- session에 값을 유지하는 방법, 30분동안 유지한다. -->
	<session-config>
		<session-timeout>30</session-timeout>
	</session-config>
```

## 로그인 구현

### 작업지시서

1. 로그인 처리 - 프로시저 활용 - 세션 적용 - MyBatis활용
2. 테이블 설계 - 시험과목에 대한 테이블 추가 - 시험응시에 대한 테이블 추가
3. 화면정의서 작성 - 시험응시에 대한 화면 그리기 - 화면과 DB테이블을 비교해 보면서 테이블 설계에 대한 타당성\(유효성\) 체크하기   누락된 컬럼이 있는지, 관계가 올바른지, 

### 테이블 관계

* 회원 - 시험응시 테이블 관계
* 시험과목 - 회원 테이블 관계
* 시험과목 - 시험응시 테이블 관계
* 관계 형태  - 1:n, n:m, 1:1 - PK와 FK선택과 확인
* 관계형태에 따라 조인 대상 테이블이 결정된다.

### 순서도

![](../../../.gitbook/assets/mvc.png)

```java
package com.di;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.springframework.web.servlet.ModelAndView;
import org.springframework.web.servlet.mvc.AbstractController;
import org.springframework.web.servlet.mvc.Controller;

//web용 시물레이션--main메서드 없이 url로 웹서비스를 제공한다.
//request, response를 누릴 수 있다. doGet이 아니더라도
//외부 서버와 통신 또는 처리 결과에 대한 자원(json, xml)을 공유할 수 있다.
public class SonataController implements Controller {
//public class SonataController extends AbstractController {

	//Controller 인터페이스에서의 오버라이드 메서드
	@Override
	public ModelAndView handleRequest(HttpServletRequest request, HttpServletResponse response) throws Exception {
		// TODO Auto-generated method stub
		return null;
	}

	/*//AbstractController 추상클래스에서의 오버라이드 메서드 
	@Override
	protected ModelAndView handleRequestInternal(HttpServletRequest request, HttpServletResponse response)
			throws Exception {
		// TODO Auto-generated method stub
		return null;
	}*/

}
```

```java
package com.di;

import org.springframework.beans.factory.BeanFactory;//spring-bean.jar
import org.springframework.beans.factory.xml.XmlBeanFactory;
import org.springframework.context.ApplicationContext;//spring-context.jar
import org.springframework.context.support.ClassPathXmlApplicationContext;
import org.springframework.core.io.FileSystemResource;
import org.springframework.core.io.Resource;

//request, response를 사용할 수 없다.
//서버와 통신하지 않는, local로 실행되므로
//웹서비스를 제공할 수 없으므로 안드로이드와 연계가 불가능하다.
//자바는 web.xml을 지원하지 않기 때문에 io로 읽고, 쓰기를 해야하는데 속도가 느리고, 서버에 부담을 준다.
//그러므로 자바로 web서비스를 하지말자
//spring-core.jar가 관리하는 ApplicationContext, BeanFactory가 bean을 관리한다.
//<bean id="" class|type=""/>로 관리한다. --type인 경우에는 추상클래스, 인터페이스까지 올 수 있다.
public class SonataSimulation {
	
	//자바 어플리케이션에서는 톰캣서버를 기동하지 않으므로 xml문서를 스캔하지 않기 때문에 직접 가져와야한다.
	//직접 스캔하기때문에 setter메서드가 필요없이 id로 직접 접근한다.
	//생성자 객체 주입법은 필요할까?
	//setter객체 주입법 코드는 자바에서 처리하고, 생성자 객체 주입법 코드는 xml에서 처리한다.
	//기존에는 VO class를 직접 만들어 사용했다. 기본적으로 멤버변수가 private이므로 접근하기 위해 setter, getter메서드로 활용했다.
	//이 변수를 별도로 초기화 하기 위해 값을 결정하는 클래스에서 직접 인스턴스화해서 set메서드로 값을 초기화하거나 생성자 파라미터에 담아 초기화한다.
	//dVO.setViewName("xxx.jsp") - new DeptVO("10", true, "xxx.jsp">
	//setter객체 주입법은 동종간 처리시 사용되고 - 권장사항
	//생성자 객체 주입버은 이종간 처리시 사용한다. - 자바와 myBatis, 자바와 Oracle이런 처리
	//결론 : xml과 xml사이에서도 객체 주입을 처리할 수 있다. 지원한다.
	/*
	 * ArrayList al = new ArraList();
	 * ArrayList al = null;
	 * ArrayList al = 타입.methodA();
	 * 나는 인스턴스화를 할 수 있다.
	 */
	public static void main(String[] args) {
		ApplicationContext context = new ClassPathXmlApplicationContext("com\\di\\sonataBean.xml");
		Sonata myCar = (Sonata)context.getBean("myCar");
		
		Resource resource = new FileSystemResource("C:\\workspace_sts3\\spring3\\src\\main\\java\\com\\di\\sonataBean.xml");
	    BeanFactory factory = new XmlBeanFactory(resource);//가운데 줄은 depricated대상이다.
	    Sonata herCar = (Sonata)factory.getBean("herCar");
	    Sonata himCar = (Sonata)factory.getBean("himCar");
	    
	    Sonata gnomCar = new Sonata();
	    
	    System.out.println("myCar : "+myCar);
	    System.out.println("herCar : "+herCar);
	    System.out.println("himCar : "+himCar);
	    System.out.println("himCar : "+gnomCar);

	}

}
```

