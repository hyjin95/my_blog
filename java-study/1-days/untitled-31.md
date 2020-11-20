---
description: 2020.11.20 - 68일차
---

# 68 Days -

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5
* 사용 서버 - WAS : Tomcat

## 필기

### 학습목표

* include의 두 가지 방법에 대해 말하고 코딩 할 수 있다. - 액션태그 : &lt; jsp:include page=" " flush=false /&gt; - 다이렉티브: &lt;%@ include file=" " %&gt;
* forward에 대해서 설명하고 활용할 수 있다. - select - req.setAttribute\(" ", 값\); - req.getAttribute\(" "\);
* ajax에서 제공하는 비동기통신을 활용해 자동갱신 처리를 할 수 있다. --오늘 실습주제
* ajax로 처리하는 경우, include, forward로 처리해야하는 경우를 구분할 수 있다.

### 개발패턴의 종류

1. JSP - JSP
2. JSP - Action - JSP
3. Action - JSP
4. Action - JSP - Action - JSP

## 뉴스 : 준비

### 기능

* 뉴스 등록
* 뉴스 녹록 조회\(조건검색\)
* 뉴스 정보 수정
* 뉴스 삭제

### UI협업 : include

* 화면의 템플릿을 패턴화 해서 분업이 가능하게 한다. - main, header, footer, menu, ..... - 화면의 모듈화

## 뉴스 : 코드

### 코드 작성시

* 요청을 서블릿에서 분리하다보면 if문으로 경우를 너무 많이 나눠야 하는 상황이 발생한다. 코드가 길어져 분석이 어려워지고, 직관적이지 않은 코드가 된다.
* 자체적으로 반복되는 코드나, 합칠 수 있는 부분을 직접 합친다. 자동화 프로그램을를 같이 사용한다\(IoC\). ex\) Spring F/W

### 전체조회, 상세조회

* Q. 서블릿과 logic의 메서드를 하나로 합칠것인가 나눠야 할까?
* A. 목적에 따라 다르다. - 관리 : 나눠야한다. - 코드 경량 : 합친다.
* Servlet에서 받은 요청은 분리하고, Logic클래스에서는 동일한 메서드를 호출하자.
* 파라미터로 조건을 구분하면 된다. - 전체조회는 0과같은 의미없는 값을 파라미터로 넘겨 전체 select하게 하고, - 상세조회는 화면에서 사용자로부터 입력받은 조건값을 파라미터로 넘겨 조건 select하게 한다.
* FrontController.java\(Servlet\) - getNewsList\( \), getNewsDetail\( \)
* NewsLogic - geNewsList\( \)

```markup
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>자동 갱신 처리 - 샘플화면</title>
<%@ include file="/common/easyUI_common.jsp" %>
<!-- 좌표값으로 접근하기 위한 CSS 속성 position: absolute -->
<style type="text/css">
	div #d_news{
		position: absolute;
	}
</style>
<script type="text/javascript">
	function autoReload(){
		alert("autoReload 호출");
	}
</script>
</head>
<body>
<script type="text/javascript">
	$(document).ready(function(){
		function start(){
			watch = setInterval(autoReload, 3000)//함수이름, 시간정보ms(3초)
		}
		function stop(){
			setTimeout(function(){
				clearInterval(watch);
			}, 10000);
		}
		start();
		stop();//stop을 호출하지않으면 무한루프
	});
</script>
<h3>자동 갱신 페이지 구현</h3>
<!-- 뉴스 목록이 출력될 위치 (좌표값을 이용해 원하는 위치에 작업한다.-CSS) -->
<div id = "d_news" width="250px" height="50px"></div>
</body>
</html>
```

