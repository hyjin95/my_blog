---
description: 2020.10.20 45 일차
---

# 45 Days - URI

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261 : Oracle.com
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5
* 사용 FrameWork - JQuery

## 복습

### HTML tree구조

* HTML파일을 브라우저가 DOMtree형식으로 변환한다. - 모든 태그\(노드\)들은 id와 name으로 식별한다.
* &lt;head&gt; - 선언\(&lt;link&gt;=import\) - 404, undefine에러가 발생하는 부분 - CSS, JS작성 가능 - 화면에 보여지지 않는다. - 에러를 확인하기 힘드므로 URL 단위테스트를 통해 확인한다.
* &lt;body&gt; - &lt;table&gt; : dataSet\(java의 list, Map\) - &lt;tr&gt;과 &lt;td&gt;로 table의 면을 만들어 정보를 담을 수 있게한다. - 이렇게 만들어진 table은 양식, form이다. - &lt;tr&gt; : map, VO, &lt;td&gt; : 원시타입 변수

### HTML JSP, UI/UX솔루션

* html은 정적이다.\(반복되는 코드를 하나로 만들어 코드를 줄일 수 없다.\) - 처리 주체가 **브라우저**인 정적 페이지로 동기화를 제공하지 않는다.
* 이런 정적인 특징을 보완하기위해 JSP를 사용한다. - Java Server Page, 처리주체가 **서버**인 자바 기반 동적 페이지
* 페이시 생산성을 위해서는 UI/UX솔루션을 사용한다. - JS기반의 easyUI, xml기반의 넥사크로 등

### Tag name, id

* 해당 태그에 접근하기 위한 식별자
* name으로 접근하기 위해서는 부모태그들을 거쳐야한다.
* 이런 접근의 문제를 JQuery를 통해 보완하자.
* id 접근 : getElementById\(" "\)
* name 접근 : getElementByName\(" "\)
* 또는 접근할 태그, 노드를 &lt;form&gt;으로 감싼다. - &lt;form&gt;태그는 일괄 전송이 가능하다. - 여러개 전송에 유리하다.
* 또는 URL 쿼리스트링으로 접근한다. - xxx.html?id=test&pw=123 - 노출된다. - 주소를 가지고 링크를 걸 수 있다.
* ID : JSP의 데이터 처리 영역에서만 사용된다.
* name : JSP는 물론 컨트롤러에서도 파라미터로 사용할 수 있다.
* JSP로 만들어진 model에서 데이터를 처리하고, 응답을 View에서 화면에 처리해야하는데 이때 필요한 중간 통신다리가 컨트롤러이다.

### 전송방식

1. **get** 방식 - http프로토콜을 이용해 주소를 가지고 링크를 걸 수 있다. - 포장이 안되어 노출 된다. - URL+append하는 것이므로 주소창의 길이 한계에 부딪히는 제약이 있다. - 주소창의 크기는 브라우저에 따라 다르다.  - 데이터 조회용
2. **post**방식 - 링크를 걸 수 없어 노출이 없다. - 데이터 수정용 - get방식보다 대용량의 전송을 할 수 있다.

### http프로토콜 전송

* 클라이언트가 서버에 요청\(request\)하면 서버가 요청에 대해 응답\(response\)하는 구조 - 사용자로부터 http프로토콜을 사용해 서버에 전송한다.
* http프로토콜 : 비 상태 프로토콜
* stateless, 연결을 계속 유지하는 것이 아니라 응답을 받으면 바로 물리적인 연결을 끊어버린다. 필요할때마다 다시 연결한다.
* 많은 동시 접속자 수를 관리하기 용이하다.
* &lt;form&gt;과 같은 전송 방법으로 파라미터를 같이 전송 할 수 있다.

### 데이터의 입력과 전송

* UI에서 사용자가 입력한 값을 표준JS나 JQuery API를 이용해 서버에 전송한다. - document.getElementById\(" "\), $\("\# "\)
* 서버에서도 전송된 값을 읽기 위해서는 JAVA가 필요하다. - JSP에서 request.getParameter\(" "\)로 파라미터를 읽어온다.
* 서버가 DB에 저장하고, htm, html 마커 언어를 통해 화면에 응답한다.

## 필기

### URI

![](../.gitbook/assets/image.png)

## 데이터 소통

### InputReadTest.html, Action.jsp 설계

![&#xC124;&#xACC4;&#xB3C4;](../.gitbook/assets/1%20%2831%29.png)

후기 : 

