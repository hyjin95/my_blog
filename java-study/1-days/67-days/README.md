---
description: 2020.11.19 - 67일차
---

# 67 Days - include-forward, include : directive, 비동기 통신 : 표준-JQuery, WEB-INF : jsp접근, java와 html결정시점

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5
* 사용 서버 - WAS : Tomcat

## 복습

### 동기, 비동기

* 동기 : 요청과 결과가 동시에 처리된다. - 결과가 나올때까지 기다린다. - 단점 : 오래 걸려도 기다린다. - forward, data-options, ajax
* 비동기 : 요청과 결과가 동시에 처리되지 않는다. - 결과를 기다리는 동안 다른 작업이 가능하다. - ajax

## 필기

### JSP컴파일

* jsp는 자바코드를 갖고있는데, 서버가 이것을 읽고 처리하려면 class파일로 컴파일되어야 한다.
* 이때 필요한것이 jsp.jar와 Servelt.jar, 이 jar파일들로 컴파일을 한다.
* jsp.jar가 jsp를 jsp.java\(Servlet\)으로 컴파일 해주고,
* servlet.jsr가 jsp.java를 jsp.class로 컴파일 해준다.
* 왜냐하면, 자바코드는 Servlet의 라이프사이클 중에서 service\( \)에서만 읽고, 처리가 가능하기 때문이다. 
* jsp파일들의 경로를 찾아가 jsp.java의 메모장을 열어보면 모든 자바 코드들이 service\( \)안에 위치 한 것을 볼 수 있다.

### html, CSS, JS  : 정적

* html, CSS, JS 이 세 언어는 정적이다.
* 이 소스들은 프로젝트가 요청, 다운로드될때 plug-in이 html이라면 브라우저에게 연결하고, 브라우저가 해석해서 화면에 처리한다.
* 다운로드될때 이미 결정된다.
* 다운로드될때 변화를 줘야 할때 필요한 것이 ajax이다.

### &lt;열린태그&gt;, &lt;닫힌태그/&gt;

* html은 닫힌태그를 작성하지 않아도 에러가 발생하지 않지만 xml에서는 허용되지 않는다.
* xml은 유효한 문서 체크, 문법체크가 이뤄진다.
* 브라우저가 xml parser를 갖고있으므로

### 안드로이드에서의 자바머신

* 안드에서의 자바머신을 달빅 이라고 한다.
* 안드로이드와 시스템을 연결시켜야 웹 서비스와 모바일용 앱과의 연계가 가능해지므로 달빅과 시스템을 연결해야 한다.
* 그렇다면 서버\(tomcat\)에 대한 내용이 존재해 달빅과 소통을 할 수 있어야 한다.
* 이 통신은 http프로토콜을 사용할 수도, 소켓통신을 사용할 수도 있다.
* 자바 - 톰캣 - 달빅 - 안드로이드
* 디바이스가 달라진 자바코드는 기존에 우리가 사용하던 자바코드와는 다르다.
* 톰캣 서버는 달빅이 아닌 외부 자바에 의해 작동한다.

### 일관성

```javascript
document.getElementById("#d_result").innerText(data); --표준
$("#d_result").text(data);//JQuery
$("#d_result").html(data);//JQuery
```

* JS에서는 해당 태그가 먼저 선언되어야 접근 할 수 있지만, JQuery에서는 이 원칙을 무시하느 경우가 있다. 표준을 어기고 있는 것이다.
* 일관성이 없는 언어는 안전하지 않기때문에 허용해주더라도 원칙을 지켜서 작성하도록 하자
* 어느 UI솔루션과 연계하느냐에 따라서 상황이 다르게 적용할 수 있기 때문이다.

### 화면 템플릿을 제공하는 API

* Vue.js

## include, forward

### include와 forward의 차이점

1. 목적 - include   페이지에 대한 템플릿을 제공하는 목적 - forward   select한 정보를 req.setAttribute로 담아 화면에 정보가 유지되도록하는 것이 목적
2. 제어권 - include   제어권이 sub.jsp로 넘어가서 페이지 내용이 모두 처리되고, 그 결과를 내보내고 나면 제어권이 다시    main.jsp로 넘어온다. - forward   제어권과 같이 응답페이지 처리에 대한 책임까지도 url에 넘어간다.

### include 사용 의의

* 페이지 모듈화, 페이지 템플릿을 만들기 위함. - 화면의 재사용성을 높인다. - View에서 많이 사용된다.
* 웹에서는 페이지 이동이 일어나도 변하지 않는 부분들이 있다. - header나 footer같은 부분들 - 이러한 부분들을 매번 작성하지 않기위해 include를 사용한다.
* 공통처리에 유용하다. 코드가 간결해진다.

### include flush의 true와 false 차이

* 화면 구조상의 차이는 없다.
* 하지만 true라면, main.jsp에서 include코드 이전에 진행된 처리 화면이 브라우저에게 내보내지게 되는데, 그렇게 버퍼가 비워지고 난 다음에는 내보내진 영역의 코드를 수정해도 적용되지 않는다.
* 시점의 차이가 있다.
* false는 모든 정보를 취합해서 한번에 완성된 페이지를 내보내는 것이고, ture일 때는 페이지를 내보내면 해당 부분은 결정된 것이므로 수정이 불가능하다.

## include : 액션태그, 다이렉티브

### include : 디렉티브 \(정적\)

* **&lt;%@ page include file="url" %&gt;**
* url에는 jsp또는 Servlet이 올 수 있다. - 하지만 Servlet는 사용하지 않는다.  - 서블릿안에서 sendRedirect, forward를 사용하면 문제가 발생 할 수 있다. - 화면으로서의 jsp만 url을 건다.
* 소스가 하나로 관리된다. - JS와 CSS - 둘다 정적이므로 하나로 관리한다. 소스 두개로 나눠 관리할 필요가 없다.

### include : 액션태그 \(동적\)

* **&lt;namespace : Nodename 속성 = 값 /&gt;**
* &lt;jsp:include page="xxx.jsp" flush="false" /&gt; - page에 서블릿은 권장하지 않는다. - flush의 기본값은 false이다.
* 소스가 두개로 관리되므로 변수의 공유가 불가능하다. - &lt;jsp : param&gt; 액션태그를 사용하면 sub.jsp에서 getParameter함수로 파라미터를 받아올 수 있다.
* 동적처리가 가능하다. -  매번 메인 페이지가 호출 될때마다. 포함되는 내용을 컴파일 하므로 파라미터가 바뀌는 경우거나,     sub.jsp가 동적처리를 하는 페이지인 경우에 사용된다.
* sub.jsp가 정적페이지라면 파일의 내용이 그대로 포함된다.
* sub.jsp가 동적페이지라면 실행 결과를 포함한다.

### flush

* mian.jsp의 내용이 먼저 처리되다가 코드 진행중에 include를 만나면 페이지에 대한 제어권이 sub.jsp로 넘어가는데, true라면 버퍼에 담긴 main.jsp를 내보내고 sub.jsp를 다시 담는 것이다.

### 서블릿을 권장하지 않는 이유

* 개발에 설계에서 디자인과 로직은 분리한다. - 재사용성 - 단위테스트와 통합테스트 - 효과적인 협업
* 위와 같은 이유로 jsp에도 가급적 자바코드를 작성하지 않는다.

## include : 디렉티브로 JS 가져오기 구현

### 디렉티브를 사용하는 이유

* JS와 CSS는 정적이므로, 이미 결정되어 있는 것이기 때문에 변화의 여지가 없으므로 소스를 분리해야할 필요가 없다. 소스를 하나로 관리해도 충분하기 때문에 액션태그가 아닌 디렉티브를 사용한다.

{% page-ref page="include-js.md" %}

## 비동기 통신 : JS표준, JQuery의 ajax

### 비동기 통신 : 표준

![](../../../.gitbook/assets/4%20%2836%29.png)

* main.jsp에서 B.jsp로 가는 것은 페이지 이동이다.  - 화면 전체가 B.jsp로 reload된다.
* 비동기 통신이라는 것은 url이 변하지 않으면서 보이지 않게 다녀와 화면 부분처리를 하는 것이다.
* 표준에서 비동기통신을 위한 객체를 생성해야한다. - JS표준에서는 브라우저가 제공해주는 XMLHttpRequest객체로 생성한다. - JQuery에서는 ajax를 사용하면 이 과정을 생략한다.
* 생성된 통신 객체는 통신 상태에 따라 콜백메서드를 호출한다. - 0,1,2,3,4\(완료\) 라는 상태를 서버측에서 브라우저를 통해 계속 체크한다.
* 위의 경우는 서버에서 가져온 응답을 바로 화면에 출력해야 한다. - html에 접근해야한다. - 면적이 있어야 값을 담을 수 있으므로 &lt;div&gt;태그의 위치에 접근한다.

### 표준 : 서버 응답 - List, Map, JSON, XML

* 표준에서, 서버의 응답은 XMLHttpRequest객체의 responseText또는 responseXML속성을 사용한다.
* 응답 data의 타입은 XML이거나 Text이다.
* List나 Map은 html태그안에서 data로서의 역할을 수행할 수 없기때문에 JSON이라는 dataSet이 필요한 것인데, 서버가 이를 text형태로 결정하면, 브라우저가 html코드를 해석한다. 

   html에서 JS가 가져온 JSON data는 Object이기때문에 Stringify로 String으로, JSON.parse로 다시 JSON형태로 바꿔 출력한다.

* 결정된 text를 포함해 브라우저가 화면을 읽어 보여주는 것이다.
* xml은 현대에는 잘 사용하지 않는 형태이다.

### 비동기 통신 코드 : 표준, JQuery

![](../../../.gitbook/assets/5%20%2824%29.png)

* 표준 - xhrObject = XMLHttpRequest로 생성한 통신 객체 - xhrObject.open\('Get', url, false \|\| true\)    방식, 요청할 페이지 주소, 비동기 처리 여부 - xhrObject.send\(null\);   전송시작   get방식이라면 null이여도 되지만, post방식일때에는 값이 들어있어야만 한다.    get방식으로는 url에 쿼리스트링으로 값을 넘길 수 있으므로
* JQuery - JQuery의 ajax를 사용하면 생략되는 코드가 많다.
* 표준에서의 var x는 ajax의 sucess속성의 함수 파라미터 data와 같다.

### 테스트

## WEB-INF하위 jsp파일 접근 : getServletContext\( \)

### 접근법

* WEB-INF 하위의 jsp파일은 url호출이 불가능하다. 하지만 보안문제로 jsp를 WEB-INF에 관리하는 경우가 있다. 이때 어떻게 접근해야 할까?
* getServletContext\( \)를 사용하면 서블릿에서 접근할 수 있다.
* 직접 문서를 타고 들어가 보면 있지만, url로 호출을 하려고하면 404번이 발생한다.
* server.xml에서 &lt;context&gt;는 서버가 접근할 수 있는 경로를 가르킨다.  - 해당 프로젝트에게 접근할 수 있는 경로를 가르키는 것이다.
* war파일을 서버에 배포하기 위해서도 이 context가 반드시 필요하다.
* getServletContext\( \)는 여기에 접근하는 함수다.

### 구현

* jsp를 직접부르면 404가 발생한다. jsp나 servlet을 경유해서 불러야만 하는데, servlet을 사용해보자
* 서블릿에서 특정 jsp를 바라보는 것은 if문으로 구분해서 url을 경로로 지정하는 것이다.
* 페이지 이름은 변수로 두고, Get방식을 사용해 쿼리스트링으로 테스트해본다.

{% page-ref page="web-inf-jsp-getservletcontext.md" %}

## JSP : 언어별 처리주체와 시점문제

### JAVA코드와 html의 결정 시점-1

![](../../../.gitbook/assets/9%20%282%29.png)

* 서버가 자바코드를 처리한 결과는 text/html, text/json, text/xml으로 나온다.
* mime타입의 main타입이 text이기때문에 html의 태그 사이에서 출력될 수 있는 것이다.
* 처리 시점은 요청이 들어오면 server를 찾아 서버가 먼저 java코드를 처리한다.  그 결과가 결정되고 나면, 브라우저가 결과를 포함해서 화면에 처리하는 것이다.

### JAVA코드와 html의 결정 시점-2

```markup
<%@ page language="java" contentType="application/json; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!-- application/hwp, application/excel 웹에서 볼 수 있는 ppt, excel,...이런것들도 브라우저가 해석해 줄 수 있다. -->
<%
	//json포맷 생성하기
	Gson g  = new Gson();
	Stirng imsi = g.toJson(주소번지:List<map>, List<VO>);
	out.print(imsi);
%>
<script>
	var result = JSON.stringify(imsi);
	var array = JSON.parse(result);
	var temp = "";
	for(var i=0;i<array.length;i++){//이렇게 태그안에 나타낼 수 있다.
		temp = "<b>"+array[i].empno+"</b>";
		temp = "<td>"+array[i].empno+"</td>";//테이블 영역이라면
	}
	$("#d_msg").html(temp);//웹 표현	
</script>
<div id="d_msg"></div>
```

* 이 코드에서 4-9번 라인은 어디에 위치하든, 10-20번 라인을 브라우저가 읽기 전에 서버가 처리해 결정된다.

### JSP안에 JAVA코드를 작성하는 방법

![](../../../.gitbook/assets/8%20%287%29.png)

* 3번은 선언부로서 멤버변수와 함수를 선언할 수 있지만 사용하지 않는다.
* 스크립틀릿과 익스프레션만 사용한다. - 스크립틀릿 : java코드 - 익스프레션 : 브라우저 화면에 작성할 java코드

### JAVA코드 위치에따른 결과변화 : 스크립틀릿

```markup
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
//여기 : 1
%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
</body>
</html>
<%
//여기 : 2 차이가 있을까?
%>
```

* 자바코드 작성 위치가 1번이냐, 2번이냐에 따라 다를까요?
* 아니요
* 어차피 자바코드는 처리주체가 서버이기 때문에 html을 브라우저가 읽기 전에 결정이된다.  따라서 위치에 상관없이 같은 결과가 출력될 것이다.

## ajax : 자동갱신 페이지 준비 - naver 뉴스

### 키워드

* &lt;div&gt;, JSON, JSP\(view\), JS, CSS

### table

* NEWS 테이블
* 컬럼 : PK, 제목, 기사내용, 기자, 날짜

### 화면 정의서

![&#xD654;&#xBA74; &#xC815;&#xC758;&#xC11C;](../../../.gitbook/assets/.png%20%2822%29.png)

1. top - include를 사용해 화면 템플릿으로 구성한다.
2. main : body - 페이지
3. main : menu - 메뉴 배치 - 메뉴 컨텐츠 표현
4. bottom - include를 사용해 화면 템플릿으로 구성한다. - 회사정보

### Q. include를 액션태그, 디렉티브 중 어느것으로 구현해야 할까?

* 액션태그가 아닐까..?

후기 : 비가온다 주룩주룩!! 오늘 상담을 했다. 그리고 내일은 비대면 수업인데.. 집에서 집중 잘 할 수 있길!

