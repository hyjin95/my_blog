---
description: 2020.12.07 - 79일차
---

# 79Day -

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5 - Spring
* 사용 서버 - WAS : Tomcat

###  TestController : 시험시작버튼

* 이벤트 발생시 swDesignExam.kos?과목코드 url로 proc\_swDesign에 요청을 하고,
* 결과는 List&lt;Map&gt;으로 나온다.
* 결과 처리 순서 - 기존 화면 지우기 - 새로우 화면 출력\(문제\) - 창 구분\(팝업\)

### TestController : JSON, HTML

* 시험시작 버튼을 클릭시 우리는 data를 HTML의 형태로 사용자 화면에 출력해야한다.
* 공통점 - forward
* 차이점 - JSON : 정보만\(dataSet\) - HTML : 문제지\(View\)

## Final\_project

### 공정표 및 주제선정, 업무분담

{% file src="../../.gitbook/assets/bobeat\_project.xlsx" %}

