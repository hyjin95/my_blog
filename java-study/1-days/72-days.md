---
description: 2020.11.26 - 72일차
---

# 72 Days -

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5 - Spring
* 사용 서버 - WAS : Tomcat

## QnA : 복습

### Q1. Action 인터페이스의 역할은 무엇인가요?

* 동일한 역할을 수행하는 메서드의 이름, 파라미터를 통일해 제공한다.

### Q2. Action 인터페이스에서 정의된 메서드 이름은 무엇인가요?

* execute
* returnType은 ActionForward클래스

### Q3. doGet과 doPost메서드가 일반 다른 메서드와 다른점은 무엇인가요?

* 서버로부터 req, res객체를 주입받고, 외부에서 라이프사이클을 관리받는다.

### Q4. ActionForward의 설계 목적에 대해 이야기할 수 있나요?

* 업무처리에 필요한 페이지 이름과 페이지 이동방식을 관리한다.

### Q5. FrontMVC1클래스는 일반적인 클래스 인가요? 

* 아니오
* HttpServlet을 상속받은 서블릿이다.

### Q6. DI에 대해서 그림으로 설명할 수 있나요?

![Dependency Injection](../../.gitbook/assets/1%20%2877%29.png)

* 의존성 주입

### Q7. 우리가 배운것 중에서 DI에 해당하는 클래스명은 무엇인가요?

* FrontMVC1 서블

### Q8. DI를 지원받기위해 개발자가 해야하는 일은 무엇인가요?

* 배치서술자 web.xml파일에 url-pattern등록하기

### Q9. forward와 sendRedirect메서드는 어디에서 호출하는지 설명할수 있나요?

* FrontMvc1 서블릿의 if문 안에서

### Q10. FrontMVC1클래스와 MemberController클래스는 어떻게 조립되었나요?

* 인스턴스화를 통해 FrontMVC1의 req에 값을 담아 MemberController의 execute메서드를 호출한다.
* Controller에서 처리된 ActionForward의 멤버변수를 활용해 다시 FrontMVC1에서 페이지이동한다.

### Q11. split메서드를 통해 알아낸 정보는 무엇인지, 어떻게 활용되었는지 기술하세요.

* 서블릿이 처리할 요청 url을 가져와 업무이름과 업무내용을 분리해 배열로 저장한다.
* 이 값들은 어떤 업무 Controller를 호출할 것인지, 어떤 Logic과 Dao를 호출하는지에 활용된다.

