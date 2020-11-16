---
description: 2020.11.13 - 63일차
---

# 63 Days - 페이지이동의 종류, servlet과 Jsp의 역할, 한글처리, 설계, 동기-비동기

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5
* 사용 서버 - WAS : Tomcat

## 페이지 이동

### 1. location.href : JS

* JS문법으로 스크립트 영역안에서 사용한다.

### 2. response.sendRedirect : JSP, Servlet

* JSP문법으로 스크립틀릿 영역안에서 사용한다.

### 3. ajax : JQuery

* JS, JQuery가 제공하는 함수

### 4. view.forward : JSP, Servlet

* JSP, Servlet문법
* req.getDispatcher, req.setAttribute와 set

### 5. data-options : easyui

* easyui가 제공하는 data-options에 url을 지정할 수 있다.

### 6. &lt;form action&gt; : html

* html form태그 안에서 action속성에 url을 지정한다.

### 1-5번 까지를 그룹화 해보기

* 1번 그룹 : url이 변하는 페이지 이동 - request가 초기화되어 유지되지 않는다. - location.href, sednRedirect
* 2-1번 그룹 : url이 변하지 않지만 페이지는 이동한다. - request가 유지된다. - forward
* 2-2번 그룹 : url이 변하지 않고 화면의 일부분만 변화한다. - ajax, data-options

### 서블릿

* doGet, doPost 이 두 메서드만이 HttpServletRequest, HttpServletResponse객체를 주입받아 사용할 수 있다.
* 서블릿은 JSP와 달리 서블릿 자체의 이름으로 호출할 수 없다. xxx.java로 url을 호출할 수 없다.
* 호출 주체가 브라우저 이므로 web.xml에 등록된 url-pattern으로 호출된다.
* 브라우저가 해당 url요청이 들어오면 WAS가 인터셉트해 연결된 서블릿문서를 호출한다. - 필요할때 WAS로부터 req, res객체를 주입받아 사용된다. - 콜백메서드와 같은 호출
* 서블릿 작업시 배치서술자 파일에 url-pattern 등록하는 것을 잊지않도록한다.
* request.setAttribute\(" ", 값\)으로 request객체에 data를 담아
* request.getDispatcher\(" "\)와 forward\(request, reponse\)로 응답페이지에게 넘길 수 있다. - forward를 사용하면 url은 변하지 않지만 화면은 불러온 응답페이지의 페이지이다.
* 보통 요청이 들어오면 해당 요청에 따라 java클래스를 호출해주는데, 따라서 if문이 필수적이고 업무가 다양할 수록 코드가 길어진다.

{% page-ref page="servlet-me.md" %}

### JSP

* xxx.jsp로 호출된다.
* 서블릿보다 비교적 자유롭게 html코드를 작성 할 수 있어 응답을 출력하는 페이지로 사용한다.
* 스크립틀릿 안에서 요청객체에 담긴 data를 가져 올 수 있다. - &lt;% request.getParameter\(" "\) %&gt; : String - &lt;% request.getAttribute\(" "\) %&gt; : Object, 캐스팅 연산자필수

### Sevlet, JSP의 request

* Servlet과 JSP의 request는 원본\(static\)일까 복사본일까 다른 객체일까?
* 다른 객체 - 각 servlet의 request, JSP의 request이다.
* 용도 : 값을 요청할때, forward할때, 저장소

### Map과 JSON

* 포맷, 형식이 비슷하다.
* Map\(key, value\)
* \[ { "이름" : 값 } \]
* 이런 자료구조체들은 request안에 담겨 유지될 수 있다. - session과 cookie가 없더라도

### 어노테이션 : @

* Web.xml에 url매핑하는 것과 같은 역할을 수행한다.
* 배치서술자 파일에 직접 매핑하는 방식과 병행해서 적절하게 사용한다.

### url-pattern 매핑

* 매핑되어있는 url-pattern이 포함된 요청이 들어오면 해당 Servlet이 요청을 듣는 것
* 어떤 경우에 요청을 가로채게 할 것인가? - 서블릿 : xxx.do : Servlet + java를 경유 - JSP      : xxx.jsp : Java를 경유하지 않는다.
* \*.do  - do로 끝나는 모든 요청을 인터셉트한다. - \*의 자리에 업무이름을 넣어 구분한다.

### 서블릿으로 요청을 듣는 이유

1. 인스턴스화가 가능하다.
2. xml에 url-pattern을 등록해 WAS가 안전하게 싱글톤으로 관리한다.
3. 디펜던시 인젝션을 할 수 있다. - WAS가 LifeCycle까지도 관리해주고, 객체를 주입해준다.

### JSP가 요청을 듣지않는 이유

1. 인스턴스화가 불가능 - java문서는 개발자가 정한대로 컴파일 되면서 문서 이름이 정해지는데, jsp는 서버가 이름을 만드므로 서버마다 규칙에 따라 이름이 모두 다르다. - 인스턴스화가 불가능해 재사용성이 떨어진다.

## 동기, 비동기 통신

### 동기, 비동기

* 서로 다른 페이지가 동기화 되느냐의 여부
* url이 변하는 페이지 이동은 동기, 비동기의 대상이 될 수 없다.
* forward, ajax, data-options는 동기, 비동기의 대상이 된다.
* forwar와 data-options는 동기 통신\(부분처리\)이지만 실시간은 아니다.

### ajax의 동기, 비동기

* ajax는 비동기, 동기 통신을 모두 지원한다.
* XMLHttpRequest : ajax가 통신할 수 있도록 해주는 객체 - 이 객체를 사용하려면 인스턴스화 + 메서드 생성 + 등 을 처리해야한다. - 이 일을 JQuery가 제공하는 ajax를 사용하면 JQuery가 대신 한다.

### 비동기 통신

* 멀티통신이 가능해 페이지처리가 효과적으로 이루어진다.
* 바깥 페이지 몰래 서버측에 요청할 수 있는 것
* 실시간 검색기능같이 기존페이지의 변화없이 일부분만 계속 통신이 이루어져 실시간 정보를 보여준다.
* 이를 가능하게 해주는 것이 ajax이다. - 그러므로 ajax의 url은 백엔드 파일의 url이 되어야 한다.  - 서버와 통신을 해야 실시간 정보를 보여줄 수 있다.

## 설계

![&#xC608;&#xC2DC;](../../../.gitbook/assets/1%20%2866%29.png)

* Servelt은 요청에 따라 필요한 java클래스와 연결해주는 역할을 한다. - 요청을 구분해야하므로 if문 사용이 필수적인데, 요청이 다양할 수록 코드가 길어질 수 있다.
* 주문테이블과 주문상세테이블이 1 : n 으로 관계가 정의 될떄, 사용자가 주문시 두 테이블에 모두 insert되어야 한다. 
* 트랜잭션 처리로 둘다 1이 반환되어야 commit되어야 한다. 이 과정을 처리하기 위해서는 Dao에 각 테이블에 insert하기 위해 메서드가 2개 필요하다.
* Logic클래스는 두 메서드를 묶는 역할, Dao에서는 sql연결 메서드를 정의한다.
* 작업이 여러 문서들을 돌아다녀야 하므로 일치하는 값을 갖는 변수들은 이름을 통일한다. 
* 업무 요청은 아주 다양할 것이고 모든 업무가 연결 되어 있다. - 회원 -&gt; 주문 -&gt; 결제 -&gt; 배송

## 한글처리

### Get과 Post의 한글인코딩 : UTF-8

* get방식은 서버.xml에 인코딩 구문을 넣어주면 되지만, Post 방식은 직접 구문을 작성해야한다.

후기 : 근처 학원이 부도가 나서 새로운 학생분들이 오신다고 하신다. 나였으면 적응할 수 있었을까 걱정된다.

