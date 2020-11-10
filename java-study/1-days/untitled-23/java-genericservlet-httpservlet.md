# Java, GenericServlet, HttpServlet

### A.java : JAVA

![NullPointerException](../../../.gitbook/assets/3%20%2840%29.png)

```java
package com.basic;

import java.io.IOException;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class A {//extends Object인 자바클래스이다. 서블릿아님
	
	public void doGet(HttpServletRequest req, HttpServletResponse res) {
		try {
			//톰캣서버에서 요청하는것이 아닌 java의 local system에서 요청하는 것,
			// NullPointerException이 발생할 것이다.
			res.sendRedirect("/index.jsp");//네트워크 요청이므로 예외처리 필수 - IOException
		} catch (IOException e) {
			e.printStackTrace();//디버깅메서드
		}
	}

	public static void main(String[] args) {
		A a = new A();
		HttpServletRequest req = null;
		//HttpServletRequest req1 = new HttpServletRequest();//인스턴스화가 불가능하다.
		HttpServletResponse res = null;
		a.doGet(req, res);
	}
}
```

* JAVA의 기본 상위객체인 Object를 상속하는 자바 클래스로, 서블릿이 아니다.
* 10번처럼 doGet메서드를 생성할 수는 있지만, JAVA는 main메서드로 실행되는 local서비스만을 제공새 서버를 사용하지 않는다.
* main메서드에서 req, res객체를 생성했지만 null이므로 NullPointerException이 발생한다.
* 28번의 직접 호출구문은 Servlet으로 WAS와 통신하는 경우에는 필요없다. - WAS가 알아서 콜백메서드 처럼 지정된 url-pattern요청이 들어오면 호출해준다.

## A2.java : GenericServlet

```java
package com.basic;

import java.io.IOException;

import javax.servlet.GenericServlet;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;

public class A2 extends GenericServlet {
	//이 클래스는 get방식, post방식을 구분할 수 없다.
	public void init() {
		
	}
	
	@Override
	public void service(ServletRequest arg0, ServletResponse arg1) throws ServletException, IOException {
		// TODO Auto-generated method stub

	}

	public void destroy() {
		
	}
}
```

* GenericServlet은 Servlet인터페이스를 상속받는 클래스이므로 HttpServlet의 req, res객체를 사용할 수없고, doGet\( \), doPost\( \)메서드도 구분할 수 없다.
* service메서드를 오버라이드 해서 서비스를 구현 할 수 있다. - 어떤 프로토콜을 이용하는 application인지에 따라 구현이 달라진다.

## A3.java : HttpServlet

```java
package com.basic;

import java.io.IOException;

import javax.servlet.GenericServlet;
import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class A3 extends HttpServlet {
	
	public void init() { }
	
	public void doService(HttpServletRequest req, HttpServletResponse res)
			throws ServletException, IOException{			

	}
	@Override
	public void doGet(HttpServletRequest req, HttpServletResponse res) 
		throws ServletException, IOException{
			doService(req, res);
		}
	
	@Override
	public void doPost(HttpServletRequest req, HttpServletResponse res) 
		throws ServletException, IOException{
			doService(req, res);
	}
	
	public void destroy() {	}	
}
```

* HttpServlet 클래스는 GenericServlet을 상속해 service\(\)메서드에 HTTP 프로토콜에 알맞는 동작을 수행하도록 구현한 클래스이다.
* HttpServlet을 상속받는 A3클래스는 req, res객체를 주입받아 사용할 수 있고, doGet, doPost메서드를 오버라이드 하여 방식을 구분할 수 있다.
* A3 클래스와 같이 Service메서드에 수행문을 구현해 처리부분을 정리 할 수 있다.
* HttpServletRequest, Respones객체를 주입받아 사용할 떄에는 항상 throws예외처리로 예외처리를 호출하는 곳에서 처리하도록 한다.  

