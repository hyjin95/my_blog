---
description: 2020.10.29 - 52일차
---

# 52 Days -

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5 - Visual Studio Code
* 사용 서버 - WAS : Tomcat

## 복습

### 화면

* HTML -&gt; e\(브라우저\)가 html해석 -&gt; dom 트리 구조 완료
* HTML - 처리주체 : client\(local\) - 정적 페이지 \(결정 되어있다\)
* JS -  html에게 동적 페이지 지원, 변수 지원, 함수 지원 - JS로는 화면을 그리지 않는다. 코드가 길어지므로 - JQuery를 사용해 코드를 더 줄 일 수 있다.
* CSS - html에 style지원

### 서버

* JSP, Servlet을 이용해 html과 java가 연결된다. - 처리주체 : 서버, 동적이다.\(미정\) - JSP : Servlet을 이용해 html에 자바코드를 작성 - Servlet : 자바에 html코드를 작성
* Servlet이 제공해주는 request, response객체 - 화면에 출력할 수 있는, 브라우저가 읽을 수 있게 하는 out내장객체를 지원한다.
* dataSet은 json형식을 사용

### 위치

* HTML : &lt;head&gt;, &lt;body&gt;의 차이점 - **재사용성 : &lt;head&gt;안에서 함수를 선언, &lt;body&gt;는 함수를 호출**한다. - &lt;head&gt;안에서 전역변수를 선언할 수 있다.   전역변수는 식별자\(pk\)로서 where절에 조건으로 사용할 수 있다.

