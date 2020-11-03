---
description: 2020.10.27 - 50일차
---

# 50 Days - 페이지이동2, html+js+json으로 dept목록구현, onSelect, onDblClickCell, pagination

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5
* 사용 서버 - WAS : Tomcat

## 복습

### 웹

* main메서드가 존재하지 않는다.
* 외부에서 접근할 수 있다.
* 통신으로 소통\(듣기-요청, 말하기-응답\)할 수 있다.
* 페이지 이동이 수시로 일어난다. - 요청을 유지해야한다.  - scope가 필요하다.
* 웹 scope의 기본은 page이고, 때에따라 request, session scope를 사용한다.

### scope

* page : 해당 페이지에서만 기억
* request : 요청이 유지되는 동안 기억, 페이지를 이동하더라도 URL이 바뀌지 않는다.
* session : 브라우저에 접속되어 있는 동안 서버 측에 저장 - cookie : 소비자가 선택한 값을 클라이언트 측에 저장한다. Ex\) 장바구니
* application : 애플리케이션, 서버가 실행하는 동안 저장, 서버가 부하된다.

{% page-ref page="../untitled-1/" %}

### 로컬시스템

* main메서드가 존재한다.
* 외부에서 접근할 수 없다.
* 페이지 이동이 일어나지 않는다.

### http, https

* https - 인증서를 갖는 서버
* http - 인증받지 못한 서버

### 서블릿

* JSP보다 먼저 존재하는 자바기반 Servlet, 상속받아서 Servlet파일을 만들 수 있다.
* 자바코드안에 html을 작성할 수 있다. - view에서 입력\(듣기\)을 받을 수 있다.
* 자바이므로 인스턴스화가 가능하고, scope도 사용할 수 있다.
* 자바이므로 웹에 직접 요청 할 수 없다. - 웹에 요청하기위해 URL을 갖도록해야한다. - web.xml 배치서술자 dd파일에 등록해야한다.
* req, res객체를 WAS에게서 주입받아 내장객체로 갖는다.
* 단점 - doGet, doPost 두가지 메서드만 갖는다. - 다른 메서드를 가질 수 있기는 하지만, req와 res를 사용할 수 없다. 

### 요청과 응답

1. 요청과 응답이 같은 html,URL에서 일어난다.
2. 요청과 응답이 서로 다른 html,URL에서 일어난다. - 페이지이동, URL이 변경된다. - 요청이 끊어져 유지되지않는다. - location.href\("xxx.html"\)   sendRedirect\("xxx.html"\)   AJAX - **session, cookie**를 사용하면 요청이 끊어졌다 하더라도 유지할 수 있다.

### data

* easyui API가 제공하는 datagrid, data-options
* data-grid : 양식, 껍데기
* data-options : 데이터 요청 - data-options="url:'xxx.json \|\| xxx.jsp'"
* JSP는 정적 데이터를 JSP는 동적 데이터를 읽는다. - WAS가 주체가 되어 처리
* html\(브라우저\)은 양식을 담고, JSON, JSP\(서버\)은 실질적 data를 갖는다.

### event

* java에서는 listener인터페이스를 사용해 구현하고, html에서는 JS로 구현한다.
* HTML에서는 JS없이 태그만으로는 로드와 동시에 구현되는 이벤트만 구현할 수 있다.

### Json의 형태

* \[ {"이름":숫자, "이름":"값", "이름":"값",....}, {"이름":숫자, "이름":"값", "이름":"값",....}, ... \]

{% page-ref page="json.md" %}

### 조건

* 점 조건 : =
* 선분조건 : between A and B, A에서 B까지 사이에서

## 페이지 이동2

### 요청, 출력 - 다른페이지

![](../../.gitbook/assets/1%20%2848%29.png)

* 기본형태
* 요청과 출력이 서로 다른 페이지에서 이루어진다.
* 페이지가 변경된다. URL이 바뀌므로 연결이 끊긴다.
* http프로토콜을 사용해 쿼리스트링으로 요청값을 전송할 수 있다. - http://ip:port번호/도메인/프로젝트명/폴더명/page이름 - URL이 변경되었다는 것은 page이름이 변경되는 것이다.
* 요청이 되어 jsp가 처리하고 응답을 내려보내 클라이언트 측에서 200번이 떨어지면 바로 연결이 끊긴다. - 요청시마다 새로운 session이 생성된다.
* 이때 session id를 cookie를 통해 local PC에 저장된다. - 페이지마다 session id가 다르다.

### 요청, 출력 - 같은페이지

![](../../.gitbook/assets/2%20%2837%29.png)

* 요청과 출력이 모두 같은 페이지에서 처리된다.

### include  - 같은페이지, 처리는 다른페이지

![](../../.gitbook/assets/3%20%2830%29.png)

* 현재의 문서에 다른 문서, 즉 다른 파일의 내용을 포함시켜 출력하는 것
* include를 사용하면 다른 페이지를 보이지않는 곳에서 브라우저가 스캔한다.
* 같이 출력해 보여줄 수 있다.

## HTML, JS

### html+JS 의 기준

* 테이블의 헤더 까지는 html로 구현해야 직관적이다.
* 이벤트 처리부분은 JS로 처리한다. - 다른 이벤트와의 공유 - 재사용성\(함수로 처리함으로서\) - ajax와의 연계 시에 유리하다\(정보공유\) - JSON으로 제공되는 data를 바로 사용가능하다.
* JS로 html의 id에 대해 조작 하려면 위치를 주의하자 - body영역에서 ready로 구현되는 함수들은 위치가 상관이 없다. - body영역에서 readu없이 구현하려면 반드시 해당 id의 선언, 생성이 된 뒤에 코드가 있어야 한다.

{% page-ref page="a-b-czipcodelist-html-js.md" %}

후기 : 일찍 자자 수업에 더 집중하기!

