---
description: 2020.10.22 - 47일차
---

# 47 Days - JSP, Servlet,

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5
* 사용 서버 - Tomcat

## 복습

### CSS

* HTML태그 내에 직접 사용할 수 있다. - 정적 표현 - &lt;table style = "width:60px;"&gt;
* CSS를 따로 외부에서 관리해야 동적처리가 가능해진다.

### html 객체, 선택자

* JS는 document객체를 통해 HTML노드에 접근해 브라우저에게 write한다.
* 요소 선택자는 id, name, class, tag name, 등이 있다.

## JSP, Servlet

### JSP

* HTML안에 자바코드를 사용해 동적으로 웹페이지를 생성할 수 있게 해주는 서버 쪽 페이지
* 기존에 있던 Servlet을 내장 객체로 정리해 갖고 있어 사용이 더 편리하다.
* mime타입으로 사용언어를 구분한다. - text / teml, xml, java, css, javascript
* HTML에 작성된 java코드는 Tomcat과같은 서버가 해석해 html에 접근하는 것이다. - 인스턴스화 없이도 내장 객체 document를 이용해 html에 접근할 수 있다.

### Servlet

