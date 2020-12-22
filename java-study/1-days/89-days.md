---
description: 2020.12.22 - 89일차
---

# 89 Days -

## 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org, Toad DBA Suite for Oracle 11.5 , Spring, Android Studio
* 사용 서버 - WAS : Tomcat

## 게시판

### 오라클을 경유하는 경우

* SELECT - 반환 값을 List&lt;&gt;에 담아 유지하고 forward로 응답페이지에 넘겨야한다. - @RequestMapping\("board/boardList.sp"\) : annotation, spring-boot - PropertiesMethodNameResolber : xml,  spring
* INSERT \| UPDATE \| DELETE

  - 트랜잭션의 처리 대상  
  - 결과 페이지가 boardList.jsp로 향하게 된다. : board/boardList.jsp  
  - 커밋과 롤백의 대상

### 경유하지 않는 경우

* SELECT,  조건 검색 - 페이지가 처음 열릴 때 DB를 경유하지 않는다.
* INSERT \| UPDATE \| DELETE - 트랜잭션의 처리 대상 - 결과 페이지가 boardList.jsp로 향하게 된다. : board/boardList.jsp - 커밋과 롤백의 대상

### return Type

* void : webapp하위의 jsp에 접근한다.
* ModelAndView, String : WEB-INF하위의 jsp에 접근한다.
* WEB-INF의 jsp에 접근하려면 반드시 Contorller를 거쳐야 하는 것이다. webapp에 있는 jsp는 url에서 해당 경로로 바로 접근할 수 있다.

