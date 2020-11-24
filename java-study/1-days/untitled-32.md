---
description: 2020.11.24 - 70일차
---

# 70 Days -

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5
* 사용 서버 - WAS : Tomcat

## 복습 : Login

### 로그인 구현 방법

* 1 : 표준서블릿을 이용한 모델1, xxx.jsp
* 2 : 사용자 정의 서블릿을 이용하는 모델2, xxx.mem
* 공통점 : HttpServlet, 서블릿을 사용한다.

### 필요 사항

* 로그인시 url이 변하지 않고, 사용자에게 맞는 화면이 부분 제공되어야 한다.
* 비동기 통신\(ajax\)와 변수를 사용해야한다.
* 프로시서저를 호출한 결과에 대한 값을 화면에 전달한다. - 변수 : 회원이름
* 화면까지 값을 유지, 전달해야한다. - forward
* 로그인 정보를 유지하기 위해 cookie를 사용할까 session을 사용할까?

## 로그인 : session과 cookie

### 서버의 사용자 식별

* 서버가 사용자마다 session id를 부여해 식별한다.
* 클라이언트는 cookie안에 session id를 내려받아 text로 갖는다.
* 개발자 도구로 application의 Cookies에 seesion id가 있는데 Domain이 localhost인 것을 볼 수 있다. - Domain : cookie가 생성된곳

### Session

* 서버가 관리한다.
* 캐시 메모리에 저장된다. - 캐시메모리 제조사 : AMD, Intel
* 캐시메모리는 공간이 작기때문에 저장할 수 있는 정보가 한정적이다. - FIFO, 휘발성 
* 서버에서 관리해 보안이 엄격해 접근하기 어렵다.

### Cookie

* 클라이언트\(local\)에서 text로 관리한다. - 사용자의 local pc에 저장된다.
* cookie는 응답을 통해 생성된다. - response객체가 있어야한다. - 반드시 응답을 내보내야하는데, 응답처리는 클라이언트에서도, 서버측에서도 처리할 수 있다.
* JS로 관리할 수 있다. - JQuery cookie API
* 노출되어도 괜찮은, 대용량이 될 수 있는 정보를 담는데 사용한다. - 장바구니, 찜한상품, 좋아요, 오늘하루 보지 않기, ...
* 클라이언트에서 관리되는 것들은 서버측에서 반드시 응답처리가 되어있어야 접근, 호출, 처리가 가능하다.

## 로그아웃 구현

### Session 삭제

* 전체 삭제 : session.invalidate\( \);
* 부분 삭제 : session.removeAttribute\('이름'\)

