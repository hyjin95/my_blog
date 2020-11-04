---
description: 2020.11.04 - 56일차
---

# 56 Days -

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5
* 사용 서버 - WAS : Tomcat

## 필기

### html과 jsp - 원본과 복사본

![](../.gitbook/assets/1%20%2857%29.png)

* session ID가 같으면, 브라우저는 화면에 끼워넣기를 한다. 단, html의 경우에는 메모리에 있는 것을 가져오므로 기존의 데이터를 가져온다.

## empManagerFinal - JSP로 구현

## 개발패턴

### 1. JSP -&gt; JSP

* DB를 거치지 않는 작업.

### 2. JSP -&gt; Action -&gt; JSP

* 화면\(검색요청\) -&gt; DB검색 -&gt; 검색결과화면

### 3. Action -&gt; JSP -&gt; Action -&gt; JSP

* 기본 DB전체조회 -&gt; 화면 -&gt; DB검색 -&gt; 검색결과화면 - ex\) 수정 : DB에서 기존 data를 가져와야 한다 = Action으로 시작

### 4. Action -&gt; Action -&gt; JSP

* DB입력 -&gt; DB조회 -&gt; 결과화면

