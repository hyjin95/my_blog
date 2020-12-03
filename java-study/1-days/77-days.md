---
description: 2020.12.03 - 77일차
---

# 77 Days -

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5 - Spring
* 사용 서버 - WAS : Tomcat

## 온라인 시험 사전 정의서

### 오늘의 학습목표

1. 화면\(View\)에 대한 Flow Chart를 작성한다. - 어떤 페이지가 필요한지 생각해본다. - 화면 정의서와 연결된다.
2. 업무 정의서\(규칙\)를 작성한다. - Model + Dao
3. 쿠키\(서블릿\) API를 활용할 수 있다. - 쿠키 : 정보의 유지, 클라이언트, text, response - 화면과 쿠키의 연결 - JS가 아닌 JAVA로 처리한다. - 서블릿\(jsp, servlet\)안에서 쿠키 객체를 생성한다.
4. 개발방법론\(MVC\) - 모델1, 모델2

### 온라인 시험 업무 정의서

1. 수험 번호와 이름 입력 --onLineTest.html\(View\)
2. 인증처리 - select + where + and - 수험번호 일치 and 이름 일치 - 사용자 정보를 cookie에 담아 유지한다.
3. 문제 페이지 - 첫번째 페이지는 html로 하되 2-5번째 문제 페이지는 jsp로 해야한다. - response를 활용해 사용자가 선택한 답을 내보내야 하기 때문이다. - 5번째 문제까지도 고른 답들을 유지해야하므로 url이 변하는 페이지 이동은 안된다.
4. 제출 - 제출 클릭시 페이지 이동이 발생하지 않으면, 서버와 클라이언트의 쿠키가 동기화 되지않는다.

### Page Flow Chart

![](../../.gitbook/assets/.png%20%2843%29.png)

## 웹 페이지에서 쿠키 생성하기 : jsp

### 생성코드 : cookieMake.jsp

```markup
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ page import="java.net.URLEncoder" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>웹 서버에서 쿠키 생성하기</title>
</head>
<body>
<!-- 쿠키API를 제공한다. 클래스를 제공해주는 것 -->
<%
	Cookie c_dap1 = new Cookie("c_dap1", "2");//string, string만 가용
	c_dap1.setMaxAge(60);//시험시간, 
	
	Cookie c_dap2 = new Cookie("c_dap2", "5");
	c_dap2.setMaxAge(60);
	
	Cookie c_name = new Cookie("c_name", URLEncoder.encode("김유신","utf-8"));
	c_name.setMaxAge(60);
	
	response.addCookie(c_dap1);//클라이언트에게 응답으로 내보내기, 쿠키 생성완료
	response.addCookie(c_dap2);
	response.addCookie(c_name);
%>
</body>
</html>
```

* 자바코드로 쿠키를 생성하기 위해 jsp 페이지를 활용했다.
* cookie는 text로 저장되기 때문에 \(string, string\)만 가용한다.
* 14번 코드로 인해  생성되고 60초가 지나면 cookie는 사라진다.
* 브라우저가 화면을 다운로드 할때, cookie는 결정되어 있다. 서버에서  response로 쿠키값을 보내면, client의 local에 text로 저장된다.
* 19번에서는 한글 정보를 담기위해  URLEncoder클래스를 사용했다.

### 조회코드 : cookieRead.jsp

```markup
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>쿠키에 담긴 정보 읽어오기</title>
</head>
<body>
<!-- 생성한 쿠키 꺼내 확인해보기 -->
<!-- 답이 여러개 일 것이니까 객체배열이 필요하다.
	서버가 클라이언트에게 저장되어있는 문자열을 요청하는 것 : request
	jsp페이지마다 req, res객체가 서로 다르다.-->
<%
	Cookie c_daps[] = request.getCookies();
	for(int i=0;i<c_daps.length;i++){
		if(c_daps[i].getName().equals("c_name")){
		 out.print(URLDecoder.decode(c_daps[i].getValue(),"utf-8")+", "+c_daps[i].getName());
		out.print("<br>");
		}
		else{
		out.print(c_daps[i].getValue()+", "+c_daps[i].getName());		
		out.print("<br>");
		}
	}
		//out.print(c_daps[0].getValue()+", "+c_daps[0].getName());	
%>
</body>
</html>
```

* 쿠키를 화면에 찍어보는 페이지
* 쿠키가 여러개라면 배열로 가져와야한다. 클라이언트 local에 저장되어 있는 쿠키를 가져오기 위해 서버는 request, 요청을 해야한다.
* 쿠키 값 : c\_daps\[0\].getValue\( \); __쿠키 이름 : c\_daps\[0\].getName\( \);
* 브라우저가 화면에 값을 출력할때, 쿠키는 이미 결정된 정보이므로 화면에 출력후 클라이언트에서 쿠키 값이 변경되면 반영할 수 없다. 반드시 페이지 이동\(새로고침\)이 있어야 동기화된다.
* for문으로 출력시 sessionID까지 출력되니 주의하자. 배열의 마지막에 출력된다.
* 담긴 값이 하나일때\(c\_daps\[0\]만 존재할때\) c\_daps\[0\]을 cookieDelete.jsp 페이지로 삭제한 후에 cookieRead.jsp를 실행해보면 16번에서도 sessionID가 출력된다.
* 18번에서 한글정보를 꺼내기위해 URLDecoder클래스를 사용했다.

### 확인하기

![](../../.gitbook/assets/.png%20%2844%29.png)

### 삭제코드 : cookieDelete.jsp

```markup
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>쿠키 삭제하기</title>
</head>
<body>
<%
	Cookie c_dap1 = new Cookie("c_dap1", "");
	c_dap1.setMaxAge(0);
	response.addCookie(c_dap1);
%>
</body>
</html>
```

* 이 페이지를 실행한 후, 다시 cookieRead.jsp를 새로고침해보면 삭제된 것을 확인할 수 있다.

### setMaxAge\( \)

* 괄호안에는 쿠키의 유효시간을 작성한다. --초단위
* 양수 입력시 cookie1.setMaxAge\(60\); : cookie1이라는 cookie는 생성되고 60초후에 사라진다.
* 음수 입력시에는 브라우저를 닫으면 cookie가 사라진다.

## 쿠키의 도메인

### 쿠키의 도메인

* 기본적으로 쿠키는 해당 쿠키를 생성한 서버에만 전송되어 다른 path, url을 갖는 사이트에는 전송되지 않으므로 접근, 사용할 수 없다.
* 하지만 같은 도메인을 사용하는 여러 사이트에서 쿠키 값을 유지해야하는 경우가 있다. 이럴때에 쿠키에 도메인 주소를 지정할 수 있다. - cookie1.setDomain\("도메인주소"\);

