---
description: 2020.11.02 - 55일차
---

# 55 Days - combo-searchbox를 Jsp,Servlet에 넘겨 조건으로 사용하기, get-post, JSP-Servlet

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5
* 사용 서버 - WAS : Tomcat

## 필기

### combobox, WHERE - AND, OR, 비교연산자

```javascript
$("#cb_search").combobox({
		  data:[
			   {cols:'empno', label:'사원번호'}
			  ,{cols:'ename', label:'사원이름'}
			  ,{cols:'sal'  , label:'급여'}
		  ]
	  	 ,onSelect:function(rec){//콤보박스선택 -> @param:object로서 row의 주소번지를 가짐. row.empno, row.ename
	  		alert('당신이 선택한 검색조건은 '+rec.cols);
			var url = '../getEmpList.jsp?cols='+rec.cols;
	  	 }
	  });
```

* AND : 교집합, 원소가 감소한다.
* OR    : 합집합, 원소가 증가한다. - 데이터가 방대해질 수 있어 잘 사용하지 않는다. 시간이 오래걸릴 수 있다.
* WHERE 컬럼명 비교연산자 값
* 위 코드에서는 WHERE cols = 값 이런식으로 사용할 수 있을 것이다. - cols는 사용자에 따라 변하는 data
* 콤보박스 옆에 textbox를 만들어 사용자에게서 비교할 대상이 되는 값을 받아와야 할 것이다. - WHERE cols = textbox입력값
* combobox에서 실제로 넘어가는 값은 valueField, 사용자에게 보여지는 값은 textField이다. - 넘어오는 data는 실제로 DB에 있는 컬럼명이여야 하고, 사용자에게는 text형태로 정의된 직관적인 값이여야 하기 때문이다.

### 병합

* 가로병합 : colspan = 병합할 셀 수
* 세로병합 : rowspan = 병합할 셀 수

### Server와 브라우저

![](../../.gitbook/assets/1%20%2856%29.png)

* 서버가 바라보고있는 프로젝트들
* 해당 프로젝트가 갖고있는 jar파일과 소스들을 사용해 프로젝트를 읽어 클라이언트에게 내려보낸다. - 처리주제 = 브라우저
* web.xml을 먼저 스캔하고, 브라우저에게 URL을 요청\(request\)한다.
* 성공적으로 로드, 내려받기가 완료된다면 200번을 띄운다.
* 브라우저는 한번 로드되면 새로고침되기 전에는 갱신되지 않는다. = 이미 결정되어 JAVA코드는 없다. - AJAX가 가능하게 해준다. \(부분갱신\)

## Get, Post - JSP, Servlet

| UI/UX | JS | JSP | Servlet |
| :---: | :---: | :---: | :---: |
| 화면, View | 전송, Event | 모델1 | 모델2 |

### get, post방식

* JS가 전송해주는 방법은 두가지가 존재한다.
* get : HTML의 기본 전송방식, doGet함수를 사용
* post : 다른 UI/UX제품들의 기본 전송방식, doPost함수를 사용
* Servlet의 doGet, doPost의 리턴타입은 void이다.

### Servlet doGet, doPost의 파라미터

* doGet, doPost함수는 개발자가 호출하는것이 아니라 시스템이 부른다. = 콜백메서드
* 사용자의 요청을 시스템이 JSP나 Servlet, html 등으로 처리하는데, html과 JSP은 자바코드를 사용할 수 없으므로 요청을 처리할 수 없다.  Servlet으로 요청을 처리하자
* Servlet의 get, post함수는 req, res내장객체를 파라미터로 받고있다.
* 이 객체들을 주입시켜주는 것이 바로 WAS이다.

### 쿼리스트링

* url : 'xxx.jsp?cols=값&name=값&name=값&......
* jsp에서는 request.getParameter\("cols또는 다른 name"\) 으로 전송된 값을 받을 수 있다. = 듣기 - cols : empno, ename, ...
* 이렇게 가져온 값을 where절에 조건으로 사용함으로서 동적쿼리를 완성할 수 있다.

### JSP

* 인스턴스화를 하려면 이름을 알아야하는데, JSP는 WAS마다 명령규칙이 달라 이름이 달라진다. -  이름을 알 수 없으므로 인스턴스화가 불가능하다. - 보안상에서는 유리하다.
* 함수를 선언할 수는 있지만 인스턴스화가 불가능해 사용하기 불리하므로 로직, 업무를 처리하는 용도로는 사용하지 않는다. - 자바를 인스턴스화해 사용할 수는 있으나 자체 인스턴스화가 불가능해 제어가 불가능하다.
* JSON을 만들어주는 용도 XML을 만들어주는 용도 화면을 출력해주는 용도

### Servlet

* req, res는 doGet과 doPost함수만이 주입받을 수 있는 객체이다. - 이때 등록된 Servelt class-name으로 인스턴스화를 할 수 있다.=자바를 제어할 수 있다.
* 다른 함수를 선언할 수는 있지만 req, res객체를 주입 받을 수 없으므로 의미가 없다. - doGet, doPost함수만 정의해 사용하자
* Servlet을 실행하려면 브라우저가 필요하고, 브라우저는 URL이 필요하므로 web.xml배치서술자에 url을 등록해야한다. - web.xml에 등록하면 WAS 서버제품이 싱글톤으로 관리, 객체를 주입해준다.    자원관리, 라이프사이클 관리를 해준다.
* 전송방식을 구분해주는 용도

### id, name 식별자

* ID : 처리주체가 브라우저인 클라이언트 쪽에서 사용한다. - Ajax, JS, Xml, Html, CSS에서 사용한다.
* Name : 처리주체가 WAS인 서버 쪽에서 사용한다. - request.getParameter로 받을 수 있는 값이다. - JSP, Servlet, JAVA에서 사용한다.

### request, response

* request : 요청, get, forward - 타입 변수 = request.getParameter\("name"\) --리턴타입 고려 - JSP, Servlet에서 전송된 요청을 듣기 할떄 사용한다. - 서버가 들은 것을 개발자가 가져와 사용하는 것
* response : 응답

### SQL 화면 없이 단위테스트 - 쿼리스트링

## JSP 사용

## Servlet 사용

