---
description: 2020.11.25 - 71일차
---

# 71 Days -

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5
* 사용 서버 - WAS : Tomcat

## 복습

### 페이지 이동

* forward - data가 유지되어야 하는 페이지 이동에 사용한다. - select - req.setAttribute, RequsetDispatcher와 함께 사용된다.
* sendRedirect - data가 유지되지 않아도 되는 페이지 이동에 사용한다. - insert, update, delete

## 온라인 시험 솔루션\(POJO F/W\) 설계 - Step1

### 전체 설계 분석

1. MVC패턴에 대한 설계\(POJO F/W\)
2. DB설계
3. 화면정의서
4. 디자인\(bootstrap - 반응형\)

### Part1 : Interface

* doGet메서드와 doPost메서드를 service메서드안에서 한번에 처리한다. - Controller의 execute메서드를 정의해 구분한다.
* 리턴타입을 void가 아닌 Object를 사용한다. - Controller의 execute메서드의 타입은 ActionForward클래스다. - 그러므로 execute의 리턴타입도 ActionForward클래스여야 한다. - ActionForward클래스는 getter, setter를 담당하는 클래스다.

### Part2 : 요청 접수 Servlet

* 
### Part3 : getter, setter

* 
