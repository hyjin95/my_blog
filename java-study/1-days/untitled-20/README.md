---
description: 2020.11.02 - 55일차
---

# 55 Days - combo-searchbox MyBatis 연동, get-post, JSP-Servlet, 페이지이동4-구현,forward, Dispatcher, Redirect

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

![](../../../.gitbook/assets/1%20%2856%29.png)

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

* Oracle에서 String은 ' ' 으로 인식한다. - cols=empno&keyword=7566 가능  - cols=empno&keyword=SMITH 불가능, 조건이 둘다 만족하지 않는다. - cols=ename&keyword='SMITH' 가능

{% page-ref page="sql.md" %}

## Dispatcher-forward,include & Redirect

![](../../../.gitbook/assets/diff.png)

### Servlet - requestDispatcher

* 클라이언트로부터 최초에 들어온 요청을 JSP/Servlet내에서 원하는 자원으로 전송하는 역할의 클래스 - a.jsp로 들어온 요청을 a.jsp내에서 RequestDispatcher를 사용해 b.jsp로 요청을 보낼 수 있다,
* 특정 자원에 처리를 요청하고 처리 결과를 얻어오는 기능을 수행하는 클래스 - a.jsp에서 b.jsp로 처리를 요청하고, b.jsp에서 처리한 결과 내용을 a.jsp의 결과에 포함시킬 수 있다.
* 요청을 보내는 방법 - forward - include

### HTTP - sendRedirect\( \)

* HttpServletResponse를 사용하면 sndRedirect메서드를 이용해 지정 경로 진행하도록 제어할 수 있다.
* HTTP리다이렉션을 이용하므로 브라우저에게 Response후 브라우저측에서 지정받은 요청 경로로 다시 재요청를 하는 방식이다. - 한 요청 범위안에서 처리되지 않는다. - 두번의 HTTP트랜잭션이 발생
* 단점 : 서버측에서 최초의 요청중에 처리한 내용을 리다이렉트된 요청안에서 공유할 수 없다
* 장점 : 현재 애플리케이션 이외의 다른 자원의 경로도 요청할 수 있다.

### Dispatcher - forward 메서드

![](../../../.gitbook/assets/99050c335b6b08e81d.png)

* 지정된 JSP/Servlet문서로 요청\(제어\)를 넘기는 역할.
* a.jsp로 요청했을 때 a.jsp에서 forward메서드를 사용해 requestDispatcher로 지정된 b.jsp로 요청을 넘길 수 있다.
* b.jsp가 요청을 처리해 브라우저에게 응답한다.
* 하나의 HTTP요청 안에서 동작이 처리된다.

### Dispatcher - include 메서드

![](../../../.gitbook/assets/997f37465b6d10221f.png)

* 처리 흐름을 지정된 JSP/Servlet문서로 넘겨 해당 문서가 처리한 결과를 현재 페이지에서 보여준다. - 현재페이지의 처리 결과에 포함시킨다.
* 파라미터로 req, res객체를 주입받는다.
* forward메서드와 다른점은 forward메서드는 b.jsp에서 처리, 출력까지 하지만 include로 넘어간 요청은 b.jsp가 다시 a.jsp에게 돌려줘 a.jsp가 나머지 처리와 출력\(응답\)한다는 것이다.

## 페이지 이동 4 - 코드구현, forward

### 페이지 이동4

1. location.href = get방식 - location은 JS가 제공하는 Object - 페이지 이동이 일어난다.

### location.href

* http비상태 프로토콜을 이용한 get방식으로는 값을 유지할 수 없다.
* URL이 변경되면 새로운 페이지와 새로운 연결이 이뤄지고 기존 연결은 끊어지는 것이므로 새 페이지는 이전 페이지의 기억을 갖고있지않다.
* JS가 값을 전송하는 방법 - 쿼리스트링   request.getParameter - Attribute   request.setAttribute\("이름",값\)   request.getAttribute\("이름",값\)

### request.setAttribute

* request.setAttribute\("이름", 값\)
* Object를 담을 수 있다. - List, 소나타, Map, ...
* 처리 페이지에서 값을 꺼낼때에는 getAttribute를 사용하는데, 반환값이 Object타입이므로 반드시 타입을 맞추는 과정이 필요하다. - String msg = \(String\)request.getAttribute\("이름"\);

### session

* 공간이 작고 휘발성이다. - 대용량 불가능, 캐시 메모리에 저장되므로 용량이 많으면 서버가 터지거나 느려진다.
* 서버에서 관리하므로 개발자가 마음대로 사용할 수 없다.

### 요청, 출력 - 다른페이지

![](../../../.gitbook/assets/1%20%2848%29.png)

* URL이 바뀌는 순간, 새로운 연결이므로 값이 유지 되지않는다.
* get방식\(쿼리스트링, &lt;form&gt;\)이나 Post방식으로 넘겨줘야만 유지된다.

### 요청, 출력 - 같은페이지

![](../../../.gitbook/assets/2%20%2837%29.png)

1. Dispatcher와 forward메서드를 사용해 현재페이지에서 URL변화없이 결과를 출력해보자
2. 함수안에서 datagrid를갱신 한다.

{% page-ref page="4.md" %}

참고

{% page-ref page="../untitled-16/" %}

## MyBatis로 DB연동하기 - JSP, XML

* where조건절에 필요한 정보 : cols, keyword
* cols : empno, ename, sal  -  if문으로 경우의 수를 분리한다. - 화면에서 'cols'로 넘어오므로 getEmpList.jsp에서 map으로 갈무리해 넘겨야한다.   pmap.put\("uempno","empno"\)   key값은 u가 붙는다.

{% page-ref page="mybatis-db-jsp-xml.md" %}

후기 : 낼은 영하 1도 코트 꺼내야지!

