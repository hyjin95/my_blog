---
description: 2020.10.23 - 48일차
---

# 48 Days - 구글 지도 API, 페이지 이동1,attr속성 조작, JQuery read 값 강제변환

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5
* 사용 서버 - WAS : Tomcat

## 복습

### HTML접근 함수

* document가 받는 문서는 해당 html문서이다. 다른 언어로 html에 접근하려면 html을 객체화 해야한다.
* 브라우저가 DOM 트리형식으로 html을 구성 완료해야한다.
* JQuery : ready함수
* JS : onload

## 페이지 이동 step1 - ajax

### 페이지 이동

* 새로운 페이지로 이동
* 요청 -&gt; URL변화 -&gt; 값 전달\(전송\)
* URL이 변경된다 -&gt; 연결이 끊겼다. -&gt;기존 페이지의 data을 사용할 수 없다.
* 페이지 요청 방법 -  JSP : java제공, sendRedirect\("URL"\) - JS : window.location.href="XXX.jsp" - AJAX - &lt;form&gt; - fetch

### 데이터 전송

```markup
<script
  src="https://maps.googleapis.com/maps/api/js?key=YOUR_API_KEY&callback=initMap&libraries=&v=weekly"
  defer
></script>
```

* 쿼리스트링 : ?를 작성해 시작하고, &을 사용해 여러 데이터를 보낼 수 있다. -  &갯수만큼의 종류, 타입이 담기지만 쿼리스트링으로 변환되면 String타입이 되어 CastingException이 발생할 수 있다.
* callback 메서드 :  system에서 호출한다. 이 방식을 사용할 때는 라이프사이클을 알아야한다. - callback=메서드이름
* cookie : data를 유지해주는 것

### JSON을 이용해 요청페이지에 응답하기

![](../../.gitbook/assets/6%20%2811%29.png)

* JSON의 역할 : 자바와 html, 사이에서 데이터를 주고 받을 수 있게 해주는 자바 라이브러리
* JSON을 활용해 응답을 요청페이지에서 출력해보자

{% page-ref page="json.md" %}

### AJAX

* 비동기 통신
* 페이지 변동, 새로고침 없이 실시간 동기화를 이뤄준다. - ex\)실시간 검색어, 실시간 뉴스가 네이버 홈 화면에서 session변동 없이도 계속 업데이트되는 것         검색창에 텍스트 입력시 자동완성기능이 수시로 변경되는 것
* JSP페이지로 이동했다가 다시 html에 데이터를 뿌린다.
* 처음에 html 태그에 id를 부여했더라도 처음 브라우저는 해당 html id를 스캔한다. 그 다음에 easyui, JQuery를 통해 ready함수로 구현한\(스캔이 끝난 다음에 실행되는\) id가 AJAX 비동기 통신을 하는 과정에서 서버를 갔다 오면서 easyui가 부여한 id로 덮어씌워진다..

{% page-ref page="untitled.md" %}

## 구글 지도 API 활용

### 준비

* google map API검색 - 구글 로그인 - 무료체험판 사용 - api키 발급 - javascript API 확인

{% page-ref page="api.md" %}

## Attribute속성 조작, JQuery ready로 강제변환

```markup
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>attrTest.html-속성 조작:속성초기화, 강제변환 </title>
<script type="text/javascript" src="https://www.jeasyui.com/easyui/jquery.min.js"></script>
</head>
<body>
	<script type="text/javascript">
		$(document).ready(function(){
			//dom이 구성된 다음(하단의 img3개 파일이 로드된 다음) 로드되므로 src가 밑의 주소로 덮어씌워진다
			var src = $('img').attr('src','/images/babyApech.png');//src외부에서 사용가능하게 선언하기 attr속성부여하기 (속성, 값)
		});
	</script>
	<img src="/images/gasan.jpg" border="5px" width="300px" height="200px"/>
	<img src="/images/namguro.jpg" border="5px" width="300px" height="200px"/>
	<img src="/images/dealim.jpg" border="5px" width="300px" height="200px"/>
</body>
</html>
```

* 12번에 선언된 src는 외부에서 재사용할 수 있도록 담는 변수이다.
* 처음 이 html문서가 브라우저에 로드되면 15-17번의 세 이미지가 나타날 것이다.
* 로드된 뒤에 10번 코드가 실행되기 때문에 결국 세 이미지는 JQuery로 부여한 속성으로 바뀐다. - img태그의 src속성을 해당 값으로 강제전환

후기 : 풀어지지 말고 긴장하기!

