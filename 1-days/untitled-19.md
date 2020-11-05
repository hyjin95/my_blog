---
description: 2020.11.05 - 57일차
---

# 57 Days -

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5
* 사용 서버 - WAS : Tomcat

## 복습

### JS - 절차 지향적

* &lt;body&gt; - $\(document\).ready\(function\( \) {       $\("\#id"\).datagrid\( { - } \); - dom구성이 완료된 후에 바로
* &lt;head&gt; - &lt;script&gt;     function 함수이름\( \) { } - dom구성 완료 이후, 호출시

### 문법에러와 논리에러

* 문법에러 : 컴파일에러
* 논리에러 : Exception\(java\)
* 논리에러가 더 해결하기 어렵다.

### &lt;form&gt; &lt;table&gt;

* &lt;from&gt;태그만으로는 정적인 양식이다.
* DB에서 자료를 뽑아 정보를 담아야 동기화된 동적 정보가 되는 것
* DataSet이 form태그와 동기화 되어야 한다.

### DataSet

* Tag과 Java사이에서 data를 담는 구조 - Tag는 DataSet을통해 java언어로 DB에서 data를 가져올 수 있다.
* json, xml, String이 될 수 있다.

### JAVA&JSP,Servlet 통신, 서비스

* JAVA는 Object를 상속받아 로컬통신만을 지원한다.
* JSP와 Servlet은 HttpServlet객체를 받아 웹 통신 서비스를 지원한다.
* JSP와 Servlet은 JAVA언어를 사용하고, JSP와 Servlet이 JAVA파일에 영향을 끼친다.
* JSP, Servlet과 JAVA의 역할을 구분하고 클래스를 분리해 사용해야한다.

