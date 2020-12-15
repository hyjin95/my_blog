---
description: 2020.12.15 - 84일차
---

# 84 Days -

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5 - Spring - Android Studio
* 사용 서버 - WAS : Tomcat

## MVC

### request, response

* void -&gt; ModelAndView -&gt; String
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

