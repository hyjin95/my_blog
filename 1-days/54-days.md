---
description: 2020.11.02 - 54일차
---

# 54 Days -

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5 - Visual Studio Code
* 사용 서버 - WAS : Tomcat

## 복습

### 복습 목표

1. 무엇을, 어떻게 전술세우기
2. 역할 생각하기
3. 교집합 찾아보기
4. 관계 생각하기
5. 이벤트 중심의 코드분석

### data와 html - 2

* 역할에 대해 생각해보자
* html 역할 : 보이는 화면, 단방향\(정적\)
* JS    역할 : 보이지않는 부분, html보완, 양방향\(동적\) - html과 JS는 상호보완적 관계 - 양방향 서비스를 하려면 제어문이 필수\(if, for\)
* data - 자료 : 정제되지 않은, 방대한 자료 - 정보 : 정제된, 필요한 자료
* 화면에 data를 어떻게 반영할 것인가 - dataSet, Json, xml, Servlet - easyui의 data-options = "url:'여기' " - 여기 : php, json, jsp, xml, ....

### servlet과 jsp - 3,4

* req, res내장객체를 서버로부터 주입받아 내장객체로 갖는다.
* Servlet으로는 화면을 구현하는데 불편해 JSP가 탄생
* 클라이언트 요청 - servlet - 서버 - JSP - 클라이언트에게 응답
* JSP는 html에 자바코드를 작성 - WAS를 상속
* Servelt은 자바에 html코드를 작성 - JAVA에 상속

### dataSet

* \[{"id":1, "text":"test"}\]
* 의미 있는 정보는 text 정보
* id는 유일한 값으로 DB의 PK역할, JAVA의 멤버변수 역할과 같다. 

## easyui - combobox

### Atype

* html

### Btype

* html + js

### Ctype

* js

### A-B차이

* UI 가독성
* 단방향, 양방향 서비스에 어느 타입이 더 유리한지

