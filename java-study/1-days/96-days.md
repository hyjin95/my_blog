---
description: 2020.01.04 - 96일차
---

# 96 Days -

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org, Toad DBA Suite for Oracle 11.5 , Spring, Android Studio
* 사용 서버 - WAS : Tomcat

## Spring

### 웹과 앱

* 웹 : html
* 앱 : apk

### 화면 페이지 배포 위치

* webapp &gt; board
* WEB-INF &gt; views &gt; board : url로 접근이 불가능한 배포위치
* 페이지 이동을 호출하는 구간은 Controller구간이다.
* 메서드의 파라미터 타입의 리턴 값에 따라 달라진다.
* **void** - req, res에 의존적 - webapp &gt; board
* **ModelAndView** - req, res에 의존적 - WEB-INF &gt; views &gt; board
* **String**  - req, res가 없어도 된다. - webapp &gt; board  **** return "redirect:xxx.jsp \| xxx.do"   return "forward:xxx.jsp" - forward는 서블릿 요청이 불가 - WEB-INF &gt; views &gt; board   return "board/boardList"와 같이 하면 ModelAndView와 동일한 위치

### UI와 DataSet

* JS기반  - JSON으로 처리한다.    json으로 만들어진 jsonBoardList.jsp페이지가 필요하지만,    UI에서 직접 값을 꺼내 담는 자바 코드는 필요없다. - 부트스트랩, 시멘틱UI, easyUI, 안드로이드 - 특별한 경우를 제외하면 오라클과 연동하지 않으므로 noSQL제품과 연동하는 경우가 많다.
* XMl기반 - 조회결과가 xml문서\(text/xml\)로 내보내져 xmlBoardList.jsp페이지가 필요하다. - 넥사크로, 플렉스, 트러스트폼, .... - 넥사크로에서 조회된 결과는 넥사크로에서 제공하는 DataSet객체에 담아야 한다.   화면을 지원하는 Grid와 DataSet을 매핑해 UI에 출력, 완성한다. - 넥사크로에서는 xmlBoardList.jsp를 만드는 대신 직접 자바단에서 xml포맷을 생성해주는 API를 지원한다.

### 

