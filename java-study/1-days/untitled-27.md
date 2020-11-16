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

### 비동기

* **ajax**, Easyui\(data-options\), Html\(Div\), JSP\(기존페이지 모르게\) - div는 자체 크기\(박스를\)갖고 내부에 수많은 node들을 가질 수 있다. - ajax로 해당 div에만 비동기 처리를 할 수 있다.
* 부분처리
* 결과가 나오는 동안 동시에 다른 요청을 처리 할 수 있다.
* 자원을 효율적으로 활용하는 방식이지만 시간이 걸릴 수 있다.
* 과거의 정보를 보여줄 수 있다.

## JSP - Html 출력

### 출력

* 이유 : 응답해야 하니까
* 어디에 : 브라우저에
* 어떻게 : 스크립틀릿을 이용한다.

### 1. &lt;td&gt;&lt;%out.print\( " " \)%&gt;&lt;/td&gt;

### 2. &lt;td&gt;&lt;%= " " %&gt;&lt;/td&gt;

### 스크립틀릿

* &lt;% %&gt;
* 이 안에서 선언되는 변수들은 모두 지역변수의 성격을 갖는다.
* 메서드를 선언할 수 없다. - 클래스안에 클래스를 선언하는 것은 가능하지만, \(내부클래스\) 메서드안에 메서드를 선언하는 것은 불가능하다.  - JSP가 호출되는 곳이 메서드 안에 위치할 것이므로 - Servlet의 service\( \){ 메서드 안 } 

## 필기

### 가상 DOM 

* 
