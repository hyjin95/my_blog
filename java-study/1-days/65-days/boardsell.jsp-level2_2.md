# boardSell.jsp : Level2\_2 - 서블릿으로 구현하기

기본, 공통코드

{% page-ref page="boardsell.jsp-level1-js-textnode.md" %}

{% page-ref page="boardsell.jsp-level2-servlet-js.md" %}

## boardSell.jsp : Level2\_2 - 서블릿으로 구현하기

### 화면 : 결과

![\(120 - 100\) \* 60 = 1200](../../../.gitbook/assets/level3%20%281%29.png)

* 이전 과정 화면들을 boardSell\_2.jsp와 같다.

### 코드 :  통신 객체 생성, boardSell\_3.jsp

```javascript
<script type="text/javascript">
	//비동기 통신 객체 담을 변수 선언
	var xhrObject = null;//멤버변수
	
	//비동기 통신객체 생성하기
	function createRequest(){
		try{
			xhrObject = new XMLHttpRequest();//IE 8.0이상, 사파리, 오페라, 크롬, 파이어폭스에서 생성할 때 
		}catch(trimicrosoft){
			try{
				xhrObject = new ActivityObject("Msxml2.XMLHTTP");//IE 6.0에서 생성할떄
			}catch(e){
				xhrObject = null;
			}
		}////////////////end of try
		if(xhrObject == null){
			alert("비동기 통신 객체 생성 에러");
		}
	}///////////////////통신 객체 생성 메서드
```

* 3번에서 멤버변수로 비동기 통신객체를 담을 변수를 선언한다.
* 여기서 만들어진 변수가 thread와 같은 역할을 한다. 요청을 안내한다.
* JS에서 제공하는 XMLHttpRequest\( \)함수로 객체를 생성할 수 있는데 통신에 관련된 사항이므로 예외처리를 해준다.

### 코드 :  통신객체로 요청 전송

```javascript
function getBoardSold(){
		 //비동기 통신 객체 생성하기 - JQuery를 사용하는 경우엔 필요 없다.
		 createRequest();
		 //이 요청을 처리할 url정보
		 var url = "/board/bsell.do"
		 //통신 전에 필요한 상수값 지정
		 xhrObject.open("Get", url, true)//true:비동기, false:동기
		 //onreadystatechange속성은 http요청의 상태 변화에 따라 호출되는 이벤트 핸들러이다.
		 xhrObject.onreadystatechange=sold_process;
		 //이 때 서버로 전송이 일어난다. 목적지는 boardSellAction.jsp
		 xhrObject.send(null);//전송처리
	}	
```

* 3번에서 통신객체를 생성해주는 메서드를 호출한다.
* 5번에서 서블릿 url을 지정해주고,
* 7번 코드로 통신 방식, url, 비동기-동기 여부를 결정한다.
* 10번에서 onreadystatechange속성을 사용해 통신 상태에 따라 함수를 호출 한다. - 대입연산자의 오른쪽에 오는 함수이름은 콜백메서드와 같이 상태변화에 따라 자동으로 호출된다. - onreadystatechange속성 : http요청의 상태변화에 따라 호출되는 이벤트 핸들러
* 11번에서 생성된 통신객체에 send함수를 사용해 전송처리한다.

```javascript
	//콜백함수 선언
	function sold_process(){
		alert(xhrObject.readyState);
		if(xhrObject.readyState == 4) {
			if(xhrObject.status == 200){//요청에 대한 응답 성공, 브라우저에 200번이 떳니?
				//보이지 않는 곳에서 몰래 처리하기, 내부적으로 처리하기, url은 변하지 않는다.
				var newTotal = xhrObject.responseText;//url요청해서 나온 값 가져오기, XML이라면 responseXML을 사용한다.
				alert("새로 집계된 판매량 : "+ newTotal);
				
				var boardSoldEL = document.getElementById("boardSold");
			    replaceText(boardSoldEL, newTotal);
			    
			    var costEL = document.getElementById("cost");//구매가를 감싸는 <span>태그의 주소번지를 담을 변수
			    var cost = getText(costEL);//구매가 100(text)을 담을 변수 
			    console.log("구매가 : "+cost)
			    
				var priceEL = document.getElementById("price");
			    var price = getText(priceEL);
			    console.log("판매가 : "+price)
			    
			    var margin = price-cost;
			    console.log("판매1건당 마진 : "+margin);
			    
			    var total_margin = margin*newTotal;
			    console.log("총 판매 마진 : "+total_margin);
			    
				var cashEL = document.getElementById("cash");
				replaceText(cashEL, total_margin);
			    
			}else{//200번이 아니다 = error발생
				alert("에러발생")
				return;//콜백함수 탈출
			}
		}
	}////////////////////콜백함수 
</script>
```

## boardServlet.java : 서블릿

```javascript
package com.ajax;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.apache.log4j.Logger;

public class BoardServlet extends HttpServlet {
	Logger logger = Logger.getLogger(BoardServlet.class);

	@Override
	public void doGet(HttpServletRequest req, HttpServletResponse res) 
		throws ServletException, IOException{
			logger.info("doGet 호출성공");
		    res.setContentType("text/html;charset=utf-8");
		    PrintWriter out = res.getWriter();
		    out.print(60);
		}
}
```

## web.xml : 배치서술자

```markup
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd" id="WebApp_ID" version="4.0">

<!-- log4j 환경파일 등록하기 서버가 기동된 동안에는 계속 유지된다. -->
	<context-param>
		<param-name>log4jConfigLocation</param-name><!-- 객체주입 -->
		<param-value>/WEB-INF/classes/log4j.properties</param-value><!-- 톰캣서버가 읽을 수 있게 한다. -->
	</context-param>
	
<!-- DD파일(Deployment Discriptor) = 배치서술자 -->
<!-- AJAX 실습 -->
	<servlet>
		<servlet-name>BOARDServlet</servlet-name>
		<servlet-class>com.ajax.BoardServlet</servlet-class>
	</servlet>
	<servlet-mapping>
		<servlet-name>BOARDServlet</servlet-name>
		<url-pattern>/board/bsell.do</url-pattern><!-- 주소앞에는 업무명이온다. 업무마다 구분하기위해 -->
	</servlet-mapping>
</web-app>
```

