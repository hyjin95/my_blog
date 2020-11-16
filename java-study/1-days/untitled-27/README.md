---
description: 2020.11.16 - 64일차
---

# 64 Days -

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5
* 사용 서버 - WAS : Tomcat

## 복습

### DOM

* 실제 DOM, 가상 DOM
* html안에는 &lt;head&gt; 와 &lt;body&gt;가, &lt;head&gt;안에는 JS, CSS, title &lt;body&gt;안에는 &lt;div&gt;, &lt;table&gt;, &lt;h2&gt;, ........ &lt;table&gt;안에는 &lt;tr&gt;, &lt;tr&gt;안에는 &lt;td&gt;,....
* 이런식의 트리구조

### 동기

* DB\(유지\), ajax, JSP\(DB와 연동\), Java\(DB와 연동\)
* 서로 맞추는 것
* 요청과 결과가 한 자리에서 이뤄져야한다. = 트랜잭션을 동시 처리
* 결과가 나올 때 까지 동시에 다른 요청을 처리할 수 없다.
* data와의 연결에는 대체적으로 동기를 사용한다. = Oracle을 사용하는 이유

### 페이지 이동 : 동기

* 동기 - location.href : JS, 정적처리, 서버 없이 local에서도 가능하다. - res.sendRedirect : Servlet, JSP, 동적처리, 반드시 서버가 필요하다.
* sendRedirect의 경우에는 res객체가 반드시 필요하므로 서버에게서 주입받는 것이 필수조건이다. 

### 비동기

* **ajax**, Easyui\(data-options\), Html\(Div\), JSP\(기존페이지 모르게\) - div는 자체 크기\(박스를\)갖고 내부에 수많은 node들을 가질 수 있다. - ajax로 해당 div에만 비동기 처리를 할 수 있다.
* 부분처리
* 결과가 나오는 동안 동시에 다른 요청을 처리 할 수 있다.
* 자원을 효율적으로 활용하는 방식이지만 시간이 걸릴 수 있다.
* 과거의 정보를 보여줄 수 있다.

### DTM, DataSet, JSP-JSON

* DefaultTableModel\(JAVA\)의 DataSet\(XML\)으로 JSP\(application/json\)을 넣어\(JS\) 출력할 수 있다.
* DTM은 header와 body로 이루어진다.

### JQuery를 사용하는 이유

* 크로스 브라우징 서비스를 제공하기 위함
* 크로스 브라우징 : 여러 브라우저에서도 동일한 서비스를 제공할 수 있는 것

## JSP - Html 출력과

### 출력

* 이유 : 응답해야 하니까
* 어디에 : 브라우저에
* 어떻게 : 스크립틀릿을 이용한다. - 스크립틀릿을 이용해 html의 &lt;td&gt;태그 안에 출력할 수 있다. - 스크립틀릿 : &lt;% %&gt; = 익스프레션 : &lt;%= %&gt; = JS : document.write\( \)

### Basic : &lt;td&gt;&lt;%out.print\( " " \);%&gt;&lt;/td&gt;

* 세미콜론이 필요

### 익스프레션 : &lt;td&gt;&lt;%= " " %&gt;&lt;/td&gt;

* 1번의 괄호안의 내용이 2번의 =다음에 들어오므로 2번에는 세미콜론이 필요하지않다.

### 익스프레션 : EL

```markup
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<%
	//스크립틀릿 : 지역변수, 제어문사용가능, 인스턴스화 가능,scope는 없음
	//메서드 호출만 가능
	out.print("비동기 출력");
%>
<br>
  <!-- 익스프레션이라고 읽는다. 출력문, 스크립틀릿과 비교해보자 -->
	<%="익스프레션 : 비동기 출력" %>
</body>
</html>
```

* Expression Language, 표현언어, 익스프레션 언어
* JSP의 기본문법을 보완하는 JSP의 스크립트 언어, 값을 표현때 사용한다.
* 출력결과  익스프레션 : 비동기 출력 비동기 출력

### 스크립틀릿

* &lt;% %&gt;
* 이 안에서 선언되는 변수들은 모두 지역변수의 성격을 갖는다.
* 메서드를 선언할 수 없다. - 클래스안에 클래스를 선언하는 것은 가능하지만, \(내부클래스\) 메서드안에 메서드를 선언하는 것은 불가능하다.  - JSP가 호출되는 곳이 메서드 안에 위치할 것이므로 - Servlet의 service\( \){ 메서드 안 } 
* html 태그안에서 스크립틀릿을 사용하면 안의 내용을 브라우저가 해석해 화면으로 내보내준다.
* 비동기적, 동기적 방법을 사용할 수 있다.

### JSP와 WAS

* Servlet, Jsp는 WAS가 있어야 java -&gt; class가 될 수 있다.
* html, js는 컴파일이 필요없는 문서들이므로 서버 없이도 local로 실행 될 수 있다.

## JSP출력과 URL

### &lt;td&gt;&lt;%=" "%&gt;&lt;/td&gt;

```java
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ page import="java.util.Map, java.util.List, java.util.HashMap" %>
<%
	int size = 0;
	List<Map<String,Object>> empList = null;
	//서블릿에는 request.setAttribute("empList", 주소번지);
	empList = (List<Map<String,Object>>)request.getAttribute("empList");
	if(empList!=null){
		size = empList.size();
	}
%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<table>
	<tr><th></th></tr>
	<%
	if(size>0){//데이터가 존재하면 진행
	for(int i=0;i<size;i++){//여기서 nullPointerException이 발생. 조회결과가 없을시
		if(size==1) break; //계속 돌면 안된다.
		Map rmap = empList.get(i);
	%>
		<tr><th><%= rmap.get("EMPNO") %></th></tr>
	<%
	}//////////end of for
	}else {//조회결과가 없음
	%>
		<tr><th>조회결과가 없습니다.</th></tr>
	<%
	}
	%>
</table>
</body>
</html>
```

* 28번 : 정보를 직접 박아 출력하는 것, data가 보여진다.
* data가 List라면 for문을 사용해 값을 출력해야 하는데, 조회결과가 없는 경우\(해당 페이지의 최초실행의 경우\)에는 List의 size가 0이므로 실행하면 500번, NullPointerException이 발생한다.
* 23번 : 이런 출력문을 사용할 때에는 반드시 if문으로 감싸 안전하게 코드를 작성한다. - if\(size&gt;0\){for문}
* 9-11번 : size는 JSP의 상단부분에서 미리 결정한다.
* 25번 : size가 1인 경우에는 for문을 타지 못하게,\(무한루프방지\) break를 사용해 빠져나간다. 

### &lt;table data-options="title": ,url:"xxx.do", ....&gt;

* 정보를 url을 통해 가져오는것, data가 직접적으로 보여지지 않는다.

### 테스트 방식 : easyui, BootStrap

* UI솔루션안에서는 data-grid의 안에서 data-options안에 url로 서블릿이 호출될 것이다.
* easyui와 BootStrap은 DataSet으로 JSON형식을 사용하기 때문에 서블릿에서 요청이 처리 되더라도 최종 출력 형태는 mime타입이 JSON인 jsp문서로 조립 되어야만 한다.

### 테스트 방식 : 직접출력\(getAttribute\)

* xxx.do\(Servlet\)에서 forward메서드로 JSP로 넘어간다.
* 서블릿에서 처리된 값이 JSP로 넘어가 JSP페이지에서 출력하는 것
* mime타입이 html

## 가상DOM

### 가상 DOM 

* 게임, 드라마 등에서 많이 사용하는데, 미리 그려놓고\(띄워놓고\) 불러오는 것이다.
* 속도가 빠르다.
* 부분처리에 sendRedirect를 사용하는 경우에 페이지이동\(url변화\)이 되어 메모리 관리가 효율적으로 이뤄지지 않는다. 이때 사용한다.
* 미리 띄워놓는것은 비동기적 처리를 말하며, ajax를 사용해 할 수 있다.

## html : Java변수와 JS변수, TimeLine

### Java변수

* 변수의 값이 먼저 결정된다.
* 브라우저가 로드할때 값과 태그를 같이 다운로드 한다.

### JS변수

{% page-ref page="java-js.md" %}

후기 : 

