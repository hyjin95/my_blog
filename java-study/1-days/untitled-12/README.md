---
description: 2020.10.29 - 49일차
---

# 49 Days - margin, padding, query, Servlet, Jsp,  req,res, 배치서술자 DD파일, 정적동적 data, mimetype, get, post

### 사용 프로그램

* 사용언어 : JAVA(JDK)1.8.0\_261, JS, JQuery
* 사용Tool \
  \- Eclipse : Eclipse.org\
  \- Toad DBA Suite for Oracle 11.5
* 사용 서버\
  \- WAS : Tomcat

## 복습

### 경로

```markup
<img src="../../images/babyApech.png"/>
<img src="http://192.168.0.187:9000/images/wood.jpg"/>
```

* 절대경로 : http프로토콜이름부터 파일명까지 모든 경로 전부 작성\
  \- 프로젝트이름과 같이 모든 경로 앞에 붙는 이름은 설정을 /로 바꿔 생략가능
* 상대경로 : 현재 내가 바라보는 경로부터 따져서 작성\
  \- ./ : 현재 내가 바라보는 경로\
  \- ../ : 현재 나를 바라보는 나의 상위경로

### 웹 서비스

* 제공 주체 : 서버\
  \- WAS(Tomcat 등)\
  \- 소통이 이뤄져야 한다.
* 요청 : request, 응답 : response를 내장객체로 인스턴스화없이 사용한다.\
  \- requset는 Servlet을 사용하고, response는 JSP를 사용한다.\
  \- WAS제품이 servlet에 req, res를 장착시킨다. = 의존성주입
* 받는 사람 : 사용자, 업무담당자
* 웹 서비스 환경설정\
  \- xxx.xml\
  \- 컴파일 하지 않아 버전관리를 하지 않아도 되어 편리하다.\
  \- 일괄 관리가 가능하다.

### log4j

```markup
<!-- log4j 환경파일 등록하기 서버가 기동된 동안에는 계속 유지된다. -->
<context-param>
		<param-name>log4jConfigLocation</param-name><!-- 객체주입 -->
		<param-value>/WEB-INF/classes/log4j.properties</param-value>
		<!-- 톰캣서버가 읽을 수 있게 한다. -->
	</context-param>
```

* 디버깅 능력
* 힌트를 제공해준다.\
  \- 시간, 클래스명, 라인번호, 메서드이름, select문
* Logger가 필요하다.
* apache가 제공하는 log4j.jar를 사용 프로젝트 lib안에 배포해야한다.\
  \- 배포하는 서버에서 작동되어야 하기 때문에 배포하는 프로젝트 파일안에 존재해야한다.
* 배치서술자 dd파일 안에서 WAS서버가 log4j를 읽어 사용할 수 있도록 한다.

### 서버의 유지

* 서버를 종료했다가 킬때마다 class들을 전부 컴파일한다.\
  \- 프로젝드 - build - classes
* 서버는 24시간 유지되어야한다.

## \<div>

### \<div>, 시멘틱태그

![](<../../../.gitbook/assets/1 (47).png>)

* 시멘틱 태그는 모두 Div태그와 같은 기능을 수행한다.
* 시멘틱태그를 이용하면 Div태그만으로 이루어지는 것이 아니라 의미를 갖는 공간이 되기때문에 영역안의 Object의 의미를 구분하기 수월해진다.
* 이렇게 의미를 부여한 태그(시멘틱태그)를 활용한 웹을 시멘틱 웹이라 한다.

### margin, padding

![](<../../../.gitbook/assets/2 (36).png>)

* margin\
  \- 물체 간의 빈 공간, 웹 사이트에서 볼 수 있는 여백 공간, 간격\
  \- 고정값을 줄수도, 가변값을 줄 수도 있다.
* padding\
  \- div와 div안의 Object(Node, 내용)와의 여백
* 웹사이트에서 특정 요소에 두께, 빈공간, 간격을 줄 떄 사용한다.

### Viewport

* 화면에 보이는 영역을 제어하는 기술
* 스마트기기에 보이는 영역을 기기의 실제 화면 크기로 변경하여 미디어 쿼리가 기기의 화면 크기를 정확하게 감지할 수 있도록 하기 위해서 사용
* 헤더에 올 수 있다.

### Query(쿼리)

* 컴퓨터나 기기를 사용하면서 매번 질문을 하는 작업을 질의, 쿼리 작업이라 한다.
* **미디어 쿼리**\
  \- 컴퓨터나 기기가 어떤 종류의 미디어인지, 또는 화면의 크기를 묻고, 감지해 웹사이트를 변경하는 기술\
  \- device의 종류에 따라 변화를 감지하고 처리할 때 사용

## \<datagrid>

### 주의사항

![](<../../../.gitbook/assets/3 (29).png>)

* 외부 API를 사용할 때에는 import, link한 뒤에 실행시켜 뒤에 쿼리스트링을 붙여 새로고침했을떄 모든 import가 200번이 뜨는지 확인해야한다.

### html,java - typeA, B, C

|              | typeA |     typeB    |  typeC |
| :----------: | :---: | :----------: | :----: |
|      사용      |  JS X | Html 태그 + JS | html X |
|      확장자     |  html |     html     |  html  |
|      화면      |   O   |       O      |    X   |
|      데이터     |  JSON |     JSON     |  JSON  |
|    데이터 동기화   |   X   |       X      |    X   |
| data로드 시점 지정 |   X   |       O      |    O   |

* 확장자가 html이라면 자바 코드를 사용할 수 없다.\
  \- 확장자를 JSP로 바꿔 자바코드를 사용한다.\
  \- JSP를 인스턴스화 할 수 없으므로 재사용이 불가능하다.\
  \- Servlet을 통해 해결한다.
* html은 정적, JSP, Servlet은 동적(동기화)을 구현\
  \- html의 처리주체 : 클라이언트-브라우저\
  \- JSP, Servlet의 처리주체 : 서버
* html 태그로만 data를 로드하면, 화면이 로드되자마자 data를 로드해준다.\
  \- JS없이는 시점을 정할 수 없다.
* 웹 기반이면 dataset은 모두 JSON형식을 사용한다.

{% content-ref url="../untitled-7/" %}
[untitled-7](../untitled-7/)
{% endcontent-ref %}

### datagrid

* JSP, Servlet을 이용하면 동기화된 실시간 data를 가져온다.
* JSON을 이용하면 정적, 동기화되지 않은 과거의 data를 보여준다.

### dataSet - 시점

1. html에서는 JSON이 처리한다.\
   \- 자바에서 ArrayList의 그림과 같다.\
   \- html에서 구현하면 화면 로딩이 끝났을때 바로 data를 보여준다.
2. JS\
   \- 사용할 떄를, 시점을 구분할 수 있다.\
   \- 버튼이 눌렸을 떄 data를 보여준다.

### Java의 server Servlet, JSP

* 자바는 서버를 갖고 있지 않다. local만 가능\
  \- response, request객체를 갖고 있지 않다. \
  \- Web을 구현할 수 없다.\
  \- 외부에서도 접속가능한 web을 제공하려면 URL패턴을 등록할 수 있는, 설정파일 web.xml(dd파일)에 url을 등록해야한다.(url-pattern)
* Servlet을 활용해 이를 가능하게 한다.\
  \- Servlet이 서버를 갖고있지는 않지만 요청, 응답하면서 서버를 사용할 수 있게 해준다.\
  \- 서버 : WAS(Tomcat 등)가 소켓, 스레드 관리까지도 해준다.
* 자바 + servlet으로 서버에서 data를 가져올 수 있다.
* 응답을 브라우저(태그만 인식)에 출력하기위해 print함수를 지원한다.\
  \- out.print함수를 사용해 브라우저에 태그를 작성할 수 있다.\
  \- Servlet : out.print("\<td>"+list.get("DEPTNO").toString()+"\</td>");\
  \- out : java, list : java, \<td> : html 섞여있어 분리하기 힘들다.\
  \- 태그에 " " 를 붙여주어야 해서 불편하다는 단점이 있다.
* 이를 도와주는 것이 JSP이다.\
  \- JSP : \<td><%=list.get("DEPTNO").toString( )%>\</td>
* 서블릿\
  \- 인터페이스로서 컨트롤타워의 역할(일은 하지않고 지시하는 Controllar)
* 자바\
  \- Model
* JSP\
  \- 뷰를 제공해 출력, 응답한다.(View)
* **요청 -> Servlet(req) -> Java -> Dao -> myBatis -> Oracle -> Dao -> Java -> JSP(res) ->응답**\
  \- 요청과 응답을 분리해 독립적으로 만들어 ui, 로직의 개발이 따로 이루어질 수 있도록 한다.\
  \- 독립성, 재사용성, 의존도\
  \- 수정, 관리, 개발이 용이하다.\
  \- 참고 : [https://gmlwjd9405.github.io/2018/11/04/servlet-vs-jsp.html](https://gmlwjd9405.github.io/2018/11/04/servlet-vs-jsp.html)

{% content-ref url="../untitled-10/" %}
[untitled-10](../untitled-10/)
{% endcontent-ref %}

### web.xml

* Servlet은 브라우저에서 직접 호출(인스턴스화)할 수 없다.\
  \- 자바기반이므로
* 브라우저는 URL로 Java, Servlet에 접근해 요청을 한다.
* 톰캣 서버는 java기반의 웹 서비스를 제공하는 서버이다.\
  \- 톰캣안에 servlet-api.jar, jsp-api.jar를 가지고 있다.
* URL패턴을 등록할 수 있는, 설정파일 web.xml에 sevlet class를 등록해야한다\
  \- WAS제품이 대신 인스턴스화해주고 싱글톤 패턴으로 관리까지 해준다.\
  \- WAS제품이 url-pattern에 등록된 이름으로 doGet(req, res)를 주입해준다.

{% content-ref url="web.xml.md" %}
[web.xml.md](web.xml.md)
{% endcontent-ref %}

### request, response객체

* Servlet의 내장객체로, WAS는 포함하고 있다.\
  \- HttpServletReques, HttpServletResponse객체
* request.getParameter함수로 입력값을 받아 올 수 있다.\
  \- Session이 유지되는 동안 값을 저장한다.
* 자바의 상위 객체는 Object이므로 서버와 통신하기위해 http프로토콜이 필요하다.\
  \- request와 response 객체가 필요하므로

{% content-ref url="../untitled-13/" %}
[untitled-13](../untitled-13/)
{% endcontent-ref %}

## 위치, 역할, 설계(개발방법론)

![](<../../../.gitbook/assets/4 (23).png>)

* 페이지 이동을 요청할 수 있는 방법 a,b,c는 모두 get방식이다.

### html은 전송방식을 표기한다.

* html의 태그는 UI/UX를 그리기 위함이다.\
  \- 브라우저는 태그를 인식해 화면을 출력한다.
* 태그안에는 전송방식이 있다. Post, Get\
  \- 사용자, 업무담당자 가 서버에게 전송
* data - header\
  \- header에 body가 어떤 정보를 담는지 작은 정보를 담는다. - get방식, 제약이 있다.\
  \- header에는 버전정보가 있어야 한다. 어떤 브라우저를 사용해야하는지 등

### Servlet 상속해 전송방식을 구분한다.

* HttpServlet을 상속받는 자바를 Servlet이라 한다.
* doGet, doPost함수를 지원해준다. 리턴타입 : void
* WAS Tomcat을 받으면 생성되는 JSP, Servlet파일을 사용할 프로젝트 안에 부여한다.\
  \- C:\Program Files\Apache Software Foundation\Tomcat 9.0\lib\
  \- jsp-apo.jar, servlet-api.jar

## HTTP

### HTTP

* 웹 상에서 클라이언트와 서버간의 요청, 응답으로 데이터를 주고 받을 수 있는 프로토콜
* 클라이언트가 http프로토콜을 통해 서버에게 요청하면, 서버가 응답을 전송한다.
* 서버가 요청을 수행하기 위해 필요한 행동을 표시하는 용도로 사용한다.
* Get, Post메서드를 제공\
  \- html의 \<form>에서 method="get || post"로 전송방법을 명시한다.

### Get방식

* **Select, 서버로부터 정보를 조회하기 위해 설계**된 메서드
* 정보를 body가 아닌 쿼리스트링이나 \<form>태그에 담아 전송한다.\
  \- URL에 조회 조건을 표시한다, 노출된다.
* 브라우저마다 URL길이의 제한이 있을 수 있다.
* 쿼리스트링 \
  \- URL의 끝에 ?와 이름, 값으로 쌍을 이루는 요청 파라미터\
  \- 여러개라면 &으로 연결
* 불필요한 요청을 제한하기 위해 js, css, img같은 정적 컨텐츠는 데이터가 크고 수정이 적어 처음 요청시 캐시해두고 동일 요청이 발생하면 캐시 데이터를 사용해 종종 컨텐츠 수정시에도 반영되지 않는경우가 있다.\
  \- 브라우저의 캐시 삭제로 해결

### Post방식

* **U, I, D, 리소스를 생성/ 변경하기 위해 설계**된 메서드
* 데이터를 HTTP의 body에 담아 전송한다.\
  \- 보이지 않아 get보다는 안전하지만 크롬 개발자 도구 등으로 요청 내용을 볼 수 있다.
* 길이의 제한이 없다. 대용량 전송이 가능
* 요청 header에 Content-type에 요청 데이터의 타입을 표시해야한다.\
  \- 표시하지 않으면 서버는 데이터 타입을 유추해 사용한다.\
  \- header : js, css, viewport, ...

### Get, Post 차이점

* 설계에서부터 Get은 Idempotent하고, Post는 Non-idempotent하게 되어 있다.
* Get(idempotent)\
  \- 동일한 연산, 요청을 n번 수행하더라도 동일한 결과, 응답을 출력한다.\
  \- 서버의 데이터 상태를 변경하지 않아 조회에 사용한다.\
  \- 브라우저에서 웹 페이지 요청, 게시글 읽기 등 
* Post(Non-idempotent)\
  \- 동일한 요청이더라도 응답이 항상 다를수 있다. \
  \- 서버의 상태나 데이터를 변경할떄 사용한다.\
  \- 게시글 작성시 서버에 저장, 삭제시 서버에서 삭제 등

{% content-ref url="testcontroller.java-get-post.md" %}
[testcontroller.java-get-post.md](testcontroller.java-get-post.md)
{% endcontent-ref %}

후기 : 요가매트랑 폼롤러를 사서 홈트를 해볼까?? 핸드폰 바꾼지 4일쨰!! 
