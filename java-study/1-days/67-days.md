---
description: 2020.11.19 - 67일차
---

# 67 Days -

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5
* 사용 서버 - WAS : Tomcat

## 필기

### &lt;열린태그&gt;, &lt;닫힌태그/&gt;

* html은 닫힌태그를 작성하지 않아도 에러가 발생하지 않지만 xml에서는 허용되지 않는다.
* xml은 유효한 문서 체크, 문법체크가 이뤄진다.
* 브라우저가 xml parser를 갖고있으므로

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

* &lt;%@ page include file="url" %&gt;
* url에는 jsp또는 Servlet이 올 수 있다. - 하지만 Servlet\(sendRedirect, forward\)는 사용하지 않는다. 문제가 발생 할 수 있다.
* 소스가 하나로 관리된다. - JS와 CSS - 둘다 정적이므로 하나로 관리한다. 소스 두개로 나눠 관리할 필요가 없다.

### include : 액션태그 \(동적\)

* &lt;namespace : Nodename 속성 = 값 /&gt;
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

### 코드 : step1.jsp

```markup
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<!-- 공통코드 시작 -->
<!-- 공통코드로 작성하는 이유
	 같은 프로젝트, 같은 UI/UX를 사용한다면 중복되는 코드는 한번만 작성하는게 효율적이다. 
	 jsp상에서 선언문<link>도 중복되므로 공통코드로 만들자. -->
<%@ include file="/common/easyUI_common.jsp" %>
<!-- 공통코드 종료 -->
</head>
<body>
<div id="d_result">여기</div>
<script type="text/javascript">
</script>
</body>
</html>
```

### 코드 : easyUI\_common.jsp - JS&lt;Link&gt;

```markup
<!-- 소스가 하나로 합쳐지니까 선언문이 두 번 올 필요가 없다. -->
<link rel="stylesheet" type="text/css" href="https://www.jeasyui.com/easyui/themes/default/easyui.css">
<link rel="stylesheet" type="text/css" href="https://www.jeasyui.com/easyui/themes/icon.css">
<link rel="stylesheet" type="text/css" href="https://www.jeasyui.com/easyui/themes/color.css">
<link rel="stylesheet" type="text/css" href="https://www.jeasyui.com/easyui/demo/demo.css">
<script type="text/javascript" src="https://www.jeasyui.com/easyui/jquery.min.js"></script>
<script type="text/javascript" src="https://www.jeasyui.com/easyui/jquery.easyui.min.js"></script>
```

### 화면 

![](../../.gitbook/assets/3%20%2841%29.png)

* jquery와 easyui에 대한 import페이지를 정상적으로 불러왔음을 확인 할 수 있다.

### 문서 직접 확인하기 : 경로

![&#xACBD;&#xB85C;](../../.gitbook/assets/1%20%2869%29.png)

![step1.jsp &#xBA54;&#xBAA8;&#xC7A5;](../../.gitbook/assets/2%20%2853%29.png)

* 외부에 작성된 코드가 들어와있는 것을 확인할 수 있다.

## 비동기 통신 : JS표준, JQuery의 ajax

### 비동기 통신 : 표준

![](../../.gitbook/assets/4%20%2835%29.png)

### 비동기 통신 코드 : 표준, JQuery

![](../../.gitbook/assets/5%20%2824%29.png)

## WEB-INF하위 jsp파일 접근 : getServletContext\( \)

