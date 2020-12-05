---
description: 2020.12.04 - 78일차
---

# 78 Days - onLineTest : 클래스조립, 인터페이스, xml분리, Final Project시작

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5 - Spring
* 사용 서버 - WAS : Tomcat

## 클래스 조립

### 클래스 조립의 3단계

![](../../../.gitbook/assets/.png%20%2843%29.png)

### Controller\(I\)와 ControllerMapper클래스

![](../../../.gitbook/assets/2%20%2866%29.png)

### Step2 : 1-2

1. ActionForward를 ModelAndView로 바꿔본다.
2. Action인터페이스에서 새로운 인터페이스를 세운다. Controller.java --Interface
3. SimpleUrlHandlerMapping대신 ControllerMapper 클래스를 설계해본다.
4. url : \*.sp2 -&gt; ActionServlet.java가 인터셉트하게 한다. ActionServlet = FrontServlet HttpServlet을 상속받는다.
5. Json형식을 출력하는 JsonController.java를 설계해본다.

### 작업 지시서

1. web.xml 문서에 ActionServlet 등록
2. mvc2.online.ActionServlet 추가
3. ModelAndView 클래스 추가\(모방해보기\) - addObject메서드와 setViewName추가
4. MemberController와 TestController추가 - 로그인 업무, 시험 업무
5. 업무 컨트롤러에는 응답페이지의 이름만 정하고 페이지 이동에 대한 지시는 ActionServlet에서 나간다.

{% page-ref page="untitled-38.md" %}

## Spring

### xml분리

![](../../../.gitbook/assets/3%20%2850%29.png)

### AOP프레임워크

* OOP로  Controller -&gt; Logic -&gt; Dao순으로 수직적으로 이어지는 업무를 처리 했다면,  AOP는 수평적 업무 처리를 할 수 있어 트랜잭션처리를 할수 있게 해준다.
* 예를들어 한 요청 처리가 여러 업무의 테이블에 영향을 끼친다면, \(n개 Logic -&gt; Dao\) Logic들의 처리를 묶어서 한번에 commit, rollback할 수 있다.

### &lt;Listener&gt;, ContextLoaderListener클래스

```markup
<listener>
		<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
</listener>
```

* web.xml에 작성되는 클래스로, xml문서를 여러개 작성할 수 있게 해주는 클래스이다.

## Final Project

* 3조 : 김우중, 김호철, 김환이, 양혜린, 윤주연, 한영진

{% file src="../../../.gitbook/assets/final\_project.xlsx" %}

후기 : 마지막 팀 프로젝트의 팀이 정해졌다. 각자 맡은 바를 열심히해서 많은 것을 얻어 갈 수 있는 경험이 되었으면 좋겠다.

