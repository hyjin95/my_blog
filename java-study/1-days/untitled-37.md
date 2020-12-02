---
description: 2020.12.02 - 76일차
---

# 76 Days -

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5 - Spring
* 사용 서버 - WAS : Tomcat

## 필기

### Main메서드에서의 DI

* ApplicationContext - spring-context.jar
* BeanFactory - spring-beans.jar
* 공통점 - 객체\(Bean\) 생성을 관리해준다. \(A a = null;\) - 결합도가 높아진다.

### 연결

* java : java - 동종간 연결 - Controller : Logic, Logic : Dao
* java : xml - 이종간 연결 - Dao : MyBatis, Controller : Spring
* xml : xml - 이종간 연결 - spring : MyBatis

### anntation

* @어노테이션을 작성하면 xml이 없어도 된다.

### &lt;context-param&gt;

* 서버 기동시 한번 읽고 유지된다.
* 공통적으로 필요한 부분을 처리할 때 사용한다. - 보안 인증, log4j, ...

### &lt;init-param&gt;

* 해당 url요청이 들어올때마다 새로 읽혀야 하는 정보, 그래서 서블릿 태그 안에 작성된다.

