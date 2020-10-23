---
description: 2020.10.23 - 48일차
---

# 48 Days -

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5
* 사용 서버 - WAS : Tomcat

## 복습

## 페이지 이동 step1

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



