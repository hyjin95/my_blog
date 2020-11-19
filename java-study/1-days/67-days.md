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

## include : forward,액션태그, 다이렉티브

### include와 forward의 차이점

1. 목적 - include   페이지에 대한 템플릿을 제공하는 목적 - forward   select한 정보를 req.setAttribute로 담아 화면에 정보가 유지되도록하는 것이 목적
2. 제어권 - include   제어권이 sub.jsp로 넘어가서 페이지 내용이 모두 처리되고, 그 결과를 내보내고 나면 제어권이 다시    main.jsp로 넘어온다. - forward   제어권과 같이 응답페이지 처리에 대한 책임까지도 url에 넘어간다.

### include 사용 의의

* 페이지 모듈화, 페이지 템플릿을 만들기 위함 - 화면의 재사용성을 높인다.
* 웹에서는 페이지 이동이 일어나도 변하지 않는 부분들이 있다. - header나 footer같은 부분들 - 이러한 부분들을 매번 작성하지 않기위해 include를 사용한다.
* 공통처리에 유용하다. 코드가 간결해진다.

### include flush의 true와 false 차이

* 화면상 구조의 차이는 없다.
* 하지만 true라면, main.jsp에서 include코드 이전에 진행된 처리 화면이 브라우저에게 내보내지게 되는데, 그렇게 버퍼가 비워지고 난 다음에는 내보내진 영역의 코드를 수정해도 적용되지 않는다.
* 시점의 차이가 있다.
* false는 모든 정보를 취합해서 한번에 완성된 페이지를 내보내는 것이고, ture일 때는 페이지를 내보내면 해당 부분은 결정된 것이므로 수정이 불가능하다.

