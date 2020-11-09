# JSP -Java간 인스턴스, scope

## sonataSimulation.jsp : 1,2

![](../../../.gitbook/assets/1%20%2861%29.png)

```markup
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ page import="com.basic.Sonata" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<!-- 나는 xml이다. jsp는 namespace이다. 태그이름은  useBean이다. -->
<jsp:useBean id="myCar" scope="page" class="com.basic.Sonata"/><!-- 스코프가 있는 인스턴스화 -->
<%
	myCar.speed=10;
	out.print(myCar.speed);
	
	Sonata herCar = new Sonata();
	herCar.speed = 100;
	out.print("<br>");//브라우저에 출력
	out.print(herCar.speed);//브라우저에 출력
%>
</body>
</html>
<!-- 이렇게는 사용하지 않는다. Servlet을 사용할것이므로 -->
```

* 다른 폴더 속의 자바소스코드를 사용할 수 있는 네가지 방법을 구현해본다.
* 첫번쨰 - xml형식으로 스코프를 부여할 수 있는 방식 : 12번
* 두번쨰 - java클래스 끼리의 인스턴스화 처럼 일반적인 인스턴스화 : 17번 - 스코프를 사용할 수 없다.

### Sonata.java

```java
package com.basic;

public class Sonata {
	public int speed=0;
	public void stop() {
		speed=0;
	}
}
```

## sonataSimulation.jsp : 3

```markup
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ page import="com.basic.Sonata" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<!-- 나는 xml이다. jsp는 namespace이다. 태그이름은  useBean이다. -->
<jsp:useBean id="myCar" scope="page" class="com.basic.Sonata"/><!-- 스코프가 있는 인스턴스화 -->
<%
	myCar.speed=10;
	Sonata herCar = new Sonata();
	herCar.speed = 100;

	response.sendRedirect("result.jsp");
%>
</body>
</html>
<!-- 이렇게는 사용하지 않는다. Servlet을 사용할것이므로 -->
```

* 세번째 - sendRedirect 메서드를 사용 : 22번 - null 출력

### result.jsp

```java
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ page import="com.basic.Sonata" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<!-- Sonata를 사용할 수 있을까? -->
<%
	//Sonata myCar = new Sonata();//복사본, 안돼! 절대 사용하지 말자
	Sonata himCar = (Sonata)request.getAttribute("myCar");//이거는 괜찮다.
	out.print(himCar);//null
%>
</body>
</html>
```

## sonataSimulation.jsp : 4

![](../../../.gitbook/assets/2%20%2848%29.png)

```markup
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ page import="com.basic.Sonata" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<!-- 나는 xml이다. jsp는 namespace이다. 태그이름은  useBean이다. -->
<jsp:useBean id="myCar" scope="page" class="com.basic.Sonata"/>
<%
	myCar.speed=10;
	Sonata herCar = new Sonata();
	herCar.speed = 100;

  request.setAttribute("myCar",myCar);
	RequestDispatcher view = request.getRequestDispatcher("result.jsp");
	view.forward(request,response);
%>
</body>
</html>
<!-- 이렇게는 사용하지 않는다. Servlet을 사용할것이므로 -->
```

* 네번째 - RequestDispatcher의 forward메서드를 사용 : 24-26번 - myCar 주소번지 출력

### result.jsp

```java
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ page import="com.basic.Sonata" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<!-- Sonata를 사용할 수 있을까? -->
<%
	//Sonata myCar = new Sonata();//복사본, 안돼! 절대 사용하지 말자
	Sonata himCar = (Sonata)request.getAttribute("myCar");//이거는 괜찮다.
	out.print(himCar);//null
%>
</body>
</html>
```

