---
description: 2020.10.29 - 49일차
---

# 49 Days - margin, padding, query, Servelt, Jsp, 웹 서비스, req,res객체,

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5
* 사용 서버 - WAS : Tomcat

## 복습

### 경로

```markup
<img src="../../images/babyApech.png"/>
<img src="http://192.168.0.187:9000/images/wood.jpg"/>
```

* 절대경로 : http프로토콜이름부터 파일명까지 모든 경로 전부 작성 - 프로젝트이름과 같이 모든 경로 앞에 붙는 이름은 설정을 /로 바꿔 생략가능
* 상대경로 : 현재 내가 바라보는 경로부터 따져서 작성 - ./ : 현재 내가 바라보는 경로 - ../ : 현재 나를 바라보는 나의 상위경로

### 웹 서비스

* 제공 주체 : 서버 - WAS\(Tomcat 등\) - 소통이 이뤄져야 한다.
* 요청 : request, 응답 : response를 내장객체로 인스턴스화없이 사용한다. - requset는 Servlet을 사용하고, response는 JSP를 사용한다. - WAS제품이 servlet에 req, res를 장착시킨다. = 의존성주입
* 받는 사람 : 사용자, 업무담당자
* 웹 서비스 환경설정 - xxx.xml - 컴파일 하지 않아 버전관리를 하지 않아도 되어 편리하다. - 일괄 관리가 가능하다.

## &lt;div&gt;

### &lt;div&gt;, 시멘틱태그

![](../.gitbook/assets/1%20%2847%29.png)

* 시멘틱 태그는 모두 Div태그와 같은 기능을 수행한다.
* 시멘틱태그를 이용하면 Div태그만으로 이루어지는 것이 아니라 의미를 갖는 공간이 되기때문에 영역안의 Object의 의미를 구분하기 수월해진다.
* 이렇게 의미를 부여한 태그\(시멘틱태그\)를 활용한 웹을 시멘틱 웹이라 한다.

### margin, padding

![](../.gitbook/assets/2%20%2836%29.png)

* margin - 물체 간의 빈 공간, 웹 사이트에서 볼 수 있는 여백 공간, 간격 - 고정값을 줄수도, 가변값을 줄 수도 있다.
* padding - div와 div안의 Object\(Node, 내용\)와의 여백
* 웹사이트에서 특정 요소에 두께, 빈공간, 간격을 줄 떄 사용한다.

### Viewport

* 화면에 보이는 영역을 제어하는 기술
* 스마트기기에 보이는 영역을 기기의 실제 화면 크기로 변경하여 미디어 쿼리가 기기의 화면 크기를 정확하게 감지할 수 있도록 하기 위해서 사용

### Query\(쿼리\)

* 컴퓨터나 기기를 사용하면서 매번 질문을 하는 작업을 질의, 쿼리 작업이라 한다.
* **미디어 쿼리** - 컴퓨터나 기기가 어떤 종류의 미디어인지, 또는 화면의 크기를 묻고, 감지해 웹사이트를 변경하는 기술 - device의 종류에 따라 변화를 감지하고 처리할 때 사용

## &lt;datagrid&gt;

### 주의사항

![](../.gitbook/assets/3%20%2829%29.png)

* 외부 API를 사용할 때에는 import, link한 뒤에 실행시켜 뒤에 쿼리스트링을 붙여 새로고침했을떄 모든 import가 200번이 뜨는지 확인해야한다.

### html,java - typeA, B, C

|  | typeA | typeB | typeC |
| :---: | :---: | :---: | :---: |
| 사용 | JS X | Html 태그 + JS | html X |
| 확장자 | html | html | html |
| 화면 | O | O | X |
| 데이터 | JSON | JSON | JSON |
| 데이터 동기화 | X | X | X |
| data로드 시점 지정 | X | O | O |

* 확장자가 html이라면 자바 코드를 사용할 수 없다. - 확장자를 JSP로 바꿔 자바코드를 사용한다. - JSP를 인스턴스화 할 수 없으므로 재사용이 불가능하다. - Servlet을 통해 해결한다.
* html은 정적, JSP, Servlet은 동적\(동기화\)을 구현 - html의 처리주체 : 클라이언트-브라우저 - JSP, Servlet의 처리주체 : 서버
* html 태그로만 data를 로드하면, 화면이 로드되자마자 data를 로드해준다. - JS없이는 시점을 정할 수 없다.

### dataSet - 시점

1. html에서는 JSON이 처리한다. - 자바에서 ArrayList의 그림과 같다. - html에서 구현하면 화면 로딩이 끝났을때 바로 data를 보여준다.
2. JS - 사용할 떄를, 시점을 구분할 수 있다. - 버튼이 눌렸을 떄 data를 보여준다.

### Java의 server Servlet, JSP

* 자바는 서버를 갖고 있지 않다. local만 가능 - response, request객체를 갖고 있지 않다.  - Web을 구현할 수 없다. - 외부에서도 접속가능한 web을 제공하려면 URL이 필요하고 설정파일 web.xml이 필요하다.
* Servlet을 활용해 이를 가능하게 한다. - Servlet이 서버를 갖고있지는 않지만 요청, 응답하면서 서버를 사용할 수 있게 해준다. - 서버 : WAS\(Tomcat 등\)가 소켓, 스레드 관리까지도 해준다.
* 자바 + servlet으로 서버에서 data를 가져올 수 있다.
* 응답을 브라우저\(태그만 인식\)에 출력하기위해 print함수를 지원한다. - out.print함수를 사용해 브라우저에 태그를 작성할 수 있다. - Servlet : out.print\("&lt;td&gt;"+list.get\("DEPTNO"\).toString\(\)+"&lt;/td&gt;"\); - out : java, list : java, &lt;td&gt; : html 섞여있어 분리하기 힘들다. - 태그에 " " 를 붙여주어야 해서 불편하다는 단점이 있다.
* 이를 도와주는 것이 JSP이다. - JSP : &lt;td&gt;&lt;%=list.get\("DEPTNO"\).toString\( \)%&gt;&lt;/td&gt;
* 서블릿 - 인터페이스로서 컨트롤타워의 역할\(일은 하지않고 지시하는 Controllar\)
* 자바 - Model
* JSP - 뷰를 제공해 출력, 응답한다.\(View\)
* **요청 -&gt; Servlet\(req\) -&gt; Java -&gt; Dao -&gt; myBatis -&gt; Oracle -&gt; Dao -&gt; Java -&gt; JSP\(res\) -&gt;응답** - 요청과 응답을 분리해 독립적으로 만들어 ui, 로직의 개발이 따로 이루어질 수 있도록 한다. - 독립성, 재사용성, 의존도 - 수정, 관리, 개발이 용이하다. - 참고 : [https://gmlwjd9405.github.io/2018/11/04/servlet-vs-jsp.html](https://gmlwjd9405.github.io/2018/11/04/servlet-vs-jsp.html)

{% page-ref page="untitled-10/" %}

### request, response객체

* Servlet의 내장객체로, WAS는 포함하고 있다. - HttpServletReques, HttpServletResponse객체
* request.getParameter함수로 입력값을 받아 올 수 있다. - Session이 유지되는 동안 값을 저장한다.
* 자바의 상위 객체는 Object이므로 서버와 통신하기위해 http프로토콜이 필요하다. - request와 response 객체가 필요하므로

{% page-ref page="untitled-13/" %}

## 위치, 역할, 설계\(개발방법론\)

![](../.gitbook/assets/4%20%2823%29.png)

### 1. html은 전송방식을 표기한다.

* html의 태그는 UI/UX를 그리기 위함이다. - 브라우저는 태그를 인식해 화면을 출력한다.
* 태그안에는 전송방식이 있다. Post, Get - 사용자, 업무담당자 가 서버에게 전송
* data - header - header에 body가 어떤 정보를 담는지 작은 정보를 담는다. - get방식, 제약이 있다. - header에는 버전정보가 있어야 한다. 어떤 브라우저를 사용해야하는지 등

### 2. Servlet 상속

* HttpServlet을 상속받는 자바를 Servlet이라 한다.
* doGet, doPost함수를 지원해준다. 리턴타입 : void
* WAS Tomcat을 받으면 생성되는 JSP, Servlet파일을 사용할 프로젝트 안에 부여한다. - C:\Program Files\Apache Software Foundation\Tomcat 9.0\lib - jsp-apo.jar, servlet-api.jar

후기 : 

