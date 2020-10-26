# TestController.java - Get, Post방식

## TestController.java

```java
package web.mvc;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.apache.log4j.Logger;

//Servlet파일
public class TestController extends HttpServlet {

	Logger logger = Logger.getLogger(TestController.class);
	
	private static final long serialVersionUID = 1L;
	/**************************************************************************
	 * @param : req - 톰캣 서버가 제공해주는 객체 - Servlet-api.jar
	 * @param : res - 톰캣 서버가 제공해주는 객체 - Servlet-api.jar
	 * 메서드 이름 뒤에 throws로 예외를 던지면 예외발생시 해당 메서드를 호출한 곳에서 처리한다.
	 * 이 서블릿은 톰캣서버가 싱글톤패턴으로 직접 관리한다. 내가 예외처리 할 수 없다. 불가능 - 스레드도, 통신도 톰켓이 관리
	 * doGet은 일종의 콜백메서드이다.
	 * 질문 : 나는 자바코드인데 브라우저에서 실행시키고 싶을때는 어떡해야할까?
	 * 힌트 : 나는 메인메서드가 없다.
	 * 	        나는 url도 없다.
	 * 		 나는 doGet메서드만 있다.
	 * 방법 : xml, 배치서술자 dd파일에 servlet class를 추가한다.	 * 
 	 *************************************************************************/
	//기본 전송방식
	public void doGet(HttpServletRequest req, HttpServletResponse res)
			throws ServletException, IOException
	{
		logger.info("doGet 호출성공");
		//응답객체를 통해 마임타입을 지정할 수 있고, 한글 인코딩도 추가할 수 있다.
		res.setContentType("text/html;charset=utf-8");
		PrintWriter out = res.getWriter();
		//브라우저에 쓰기 
		out.print("<b>서블릿으로 그린 웹페이지 입니다.</b>");//java에 html코드를 작성할 수 있다. servlet
	}
	//post방식으로 전송하려면 반드시 <form>이나 JS를 사용해야한다.
	public void doPost(HttpServletRequest req, HttpServletResponse res)
			throws ServletException, IOException
	{
		logger.info("doPost 호출성공");
		//응답객체를 통해 마임타입을 지정할 수 있고, 한글 인코딩도 추가할 수 있다.
		res.setContentType("text/html;charset=utf-8");
		PrintWriter out = res.getWriter();
		out.print("<b>Post전송으로 요청된 웹페이지 입니다.</b>");
	}

}
```

## web.xml

