---
description: 2020.11.16 - 64일차
---

# 64 Days - JSP에서의 html출력과 URL과 Scope, HTML에서의 Java변수, 익스프레션

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

### String과 StringBuffer, StringBuilder

* String은 setText와 비슷한 성격을 갖는다. - 문자를 덩어리로 취급해 끼어들기가 불가능하고, 기존데이터가 유지되지 않고 새로 덮여진다. - document.write도 이런 성격
* StringBuffet와 StringBuilder는 append와 성격이 비슷하다. - 문자를 하나하나 인식하므로 메모리관리에 효율적이고, 끼어들기가 가능해 기존 데이터에 이어 작성 할 수 있다.

{% page-ref page="document.wirte.md" %}

### loC, OOP, AOP

* loC : 역제어, 외부에서 제어해주는 것
* OOP : 수직관계, 상속관계 - 객체지향
* AOP : 수평관계 - 관점지향

### 파일 저장의 안전성 : WEB-INF폴더

* 프로젝트 하위의 자바 문서들은 WEB-INF하위의 classes폴더 안에 컴파일 된다. - eclipse의 경우에는 auto build API를 활용해 해당 작업을 자동으로 수행한다.
* WEB\_INF폴더 하위의 lib폴더는 외부 library\(jar\)파일들을 담는 곳이다.
* JSP파일도 WebContent폴더 밑에 두는 것보다 WEB-INF의 하위 폴더에 두는 것이 더 안전하다. - 단, WEB-INF폴더 안에 관리하게되면 url을 통한 호출을 할 수 없게된다.

## JSP

![](../../../.gitbook/assets/1%20%2867%29.png)

## JSP - Html 출력

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

### mime type에 따른 출력 변화

* mime type에 따라 확장자가 같은 JSP이더라도 출력 결과가 완전히 달라진다.
* mime type - json - UI솔루션의 dataSet에 사용되는 형식이다. \(easyui, BootStrap\)
* mime type - html - 응답을 출력하는 화면의 역할을 하는 jsp이다.

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

### Java변수과 JS변수의 위치

* JSP, Servlet에서 data가 먼저 결정되고, html 태그들이 결정된다.
* Java변수와 JS변수는 &lt;head&gt;영역에도, &lt;body&gt;영역에서도 선언될 수 있다.
* 단, &lt;body&gt;의 경우에는 반드시 변수의 선언위치가 해당 변수의 사용위치 보다 위에 작성되어야한다.
* html은 절차지향적 언어이므로

{% page-ref page="java-js.md" %}

## Scope

### DB 조회 값

* DB에서 select한 data는 List&lt;Map&gt;이나 List&lt;VO&gt;로 나온다. - VO는 타입을 맞출 필요가 없어 계산이 많이 일어나는 상황에서 사용한다.

### request

* 요청이 일어나는 동안 data를 유지한다.
* 대표적인 예시 : request.setAttirbute\("이름", 값\) - forward\(req, res\) - request.getAttribute\("이름"\) - RequsetDispatcher에서 제공하는 메서드로, JSP, Servlet에서 사용가능하다. - 서블릿에서 DB처리를하고, select된 data를 req에 담아 JSP에게 넘겨 출력할 때 사용한다.

### session

* session은 캐시메모리를 사용하므로 공간이 작아 List나 Map을 담기 어렵다.
* 주로 사용자의 id, 이름, 로그인정보 와 같은 비교적 작은 정보만을 담는 용도로 사용한다.
* 일정 시간동안 data를 유지한다.

### page와 application

* page scope는 기본이지만 WEB은 특성상 페이지 이동이 빈번하게 일어나기 때문에 사용의미가 없다.
* application은 해당 애플리케이션의 실행시점부터 종료시점까지 유지되는데, 계속 쌓이면 서버가 다운 되는 등의 일이 발생할 수 있어 위험하다.

### sendRedirect, forward와 session

* sendRedirect로 일어난 페이지 이동은 url과 request가 변하는 페이지 이동이므로 data가 유지되지 않는데, data를 유지하는 scope를 사용하면 이를 극복할 수 있다.
* session
* 단, forward메서드와 함께 사용하는 serAttribute메서드에는 session을 사용하면 forward메서드의 파라미터와 맞지않아 사용할 수 없다.

## QnA

### Q1.서버에서 결정된 Java변수를 html에서 변경할 수 있나요?

* 바꿀 수 있다.
* ajax와 for문, if문을 사용한다면 가능하다.
* JS를 활용한 비동기통신객체 API를 사용할 수 도 있는데, 이 경우에는 JQuery를 사용하는 것이 코드가 더 간결하다.

### Q2. Select를 사용한 경우에는 Servlet과 JSP중에 어디를 경유해야 하나요?

* Servlet
* Servlet을 경유해 처리된 select data를 forward메서드를 사용해 JSP가 출력하게 한다. - Java가 Servlet에게 넘겨주는 값은 return값이다. - req.setAttribute, req.getAttribute - getAttribute의 리턴타입은 Object이므로 때에따라 캐스팅 연산자가 필요하다. - forward는 정보를 유지하기위해 사용되는 메서드이다. - req의 자리에 session이 올 수 있다.
* Java에서 조회된 결과가 없어 서버가 읽어오지 못하면 NullPointerException, 500번이 발생한다.

### Q3. req.setAttribute\( \), session.setAttribute\( \), application.setAttibute 세가지 중에서 forward와 sendRedirect의 구분없이 정보를 유지 할 수 있는 것은 어느것인가요?

* scopet가 session과 application인 2, 3번

### Q4. &lt;jsp:useBean id="myCar" scope={page \|\| request \|\| session \|\| application} /&gt;에서 시간을 조정할 수 있는 scope는 무엇인가요?

* session 인 3번

후기 :  미세먼지 폭발, 내 목도 맛이가고...그나마 수요일부터 금요일까지 비가온다 그래서 다행인것 같다.

