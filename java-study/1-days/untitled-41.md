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

