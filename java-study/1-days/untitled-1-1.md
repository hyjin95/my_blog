---
description: 2020.11.09 - 59일차
---

# 59 Days -

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5
* 사용 서버 - WAS : Tomcat

## 복습

### 서버와 지역변수

* automatic Variable, local Variable
* 인스턴스화 하여 사용할 수 없다.
* 서버마다 제공해주는 클래스가 달라 클래스 이름으로 해당 변수를 호출 할 수 없다.
* 서버\(tomcat, jeus, ....\)마다 jar파일이 있으므로

## local - Web - App

### Local

* Java\(화면, 통신, 스레드, 로직까지\)
* 동기화되지 않는다.
* thread와 socket을 직접 구현해 서버를 구축해야 하므로 시간과 비용이 든다.

### Web App

* **정적** : html, JS, CSS - 처리주체인 브라우저에는 객체가 내장되어 있어 이벤트 처리가 가능하다. - 하지만 비동기 - html은 재사용성이 떨어지고 유지보수가 불리하다. -&gt; xml
* **동적** : JSP, Servlet, Java - 비동기를 보완하기위한 언어 - 통신을 이용해 정보를 동기화한다. - JSP\(View\) - 디바이스마다 다른 설정을 제공하는 반응형 웹 - Java\(로직, 제어문\) - WAS제품이 스레드 관리를 해주므로 java로 직접 스레드를 구현, 관리할 필요가 없다.

### App\(Mobile - Android, IOS\)

* Android\(google\), IOS\(apple\)
* 모바일 서비스

## JSP

### JSP

* JSP &lt; Servlet &lt; Java - JSP는 확장자가 jsp, Servlet은 확장자가 java - Java의 부모는 Object로 req, res를 지원하지 않아 직접 스레드를 구현, 관리 해야한다. - Servlet은 서버에게서 req, res를 객체주입 받아 스레드는 WAS가 관리해준다.
* mime타입에 따라 문서의 성격이 달라진다. - html, xml, json, ...등 - 서브타입이 html인 jsp문서라면 태그가 있더라도 html문서취급되어 내용만 보여진다.

### JSP와 HTML

| JSP -  text/html | HTML |
| :--- | :--- |
| &lt;% 자바소스코드 %&gt; | 자바코드 사용 불가 |
|  |  |

### &lt;% 자바소스코드 %&gt;

* 스크립틀릿
* 자바코드를 작성할 수 있다.
* 변수를 선언할 수 있지만 지역변수만 가능하다.
* 메서드 선언은 불가능
* 제어문은 사용할 수 있다.
* document.write와 같은 역할을 할 수 있다. - out.print\( \);

