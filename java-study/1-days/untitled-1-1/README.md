---
description: 2020.11.09 - 59일차
---

# 59 Days - local-Web-App, viewport-반응형웹, 스크립틀릿, 페이지이동,

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5
* 사용 서버 - WAS : Tomcat

## 필기

### 서버와 지역변수

* automatic Variable, local Variable
* 인스턴스화 하여 사용할 수 없다.
* 서버마다 제공해주는 클래스가 달라 클래스 이름으로 해당 변수를 호출 할 수 없다.
* 서버\(tomcat, jeus, ....\)마다 jar파일이 있으므로

### Servlet 라이프사이클

* 생성 : init\( \)
* 사용 : service\( \) - doGet\( \), doPost\( \)오버라이드
* 종료 : destroy\( \)

### html과 css

* css가 html안에 들어가게되면 정적이 되어버리므로 CSS는 항상 외부에서 따로 관리한다.
* 분리하는것이 표준
* html, js, css는 서버\(thread, 소켓\)없이도 단독으로 처리가 가능한 언어이다.
* 하지만 비동기, 이미 결정된 정보만 보여준다.

### Java의 웹 통신, WAS

* Java만으로는 로컬 서버만 제공한다.
* 외부에서도 접근할 수 있는 웹 서비스를 제공하려면 thread와 소켓을 직접 구현, 관리해야한다.
* 이 thread를 직접 생성, 관리하지 않도록 도와주는 것이 WAS 서버이다.
* WAS는 thread를 직접 생성, 관리해 통신을 브라우저에게 맡기고 자바는 로직, DB업무에 포커스한다.

### ajax

* 비동기통신
* 정적 페이지의 일부를 동적처리하고 싶을 때 사용하면, 페이지이동, 페이지 새로고침 없이 서버에 경유할 수 있다.

### war

* 프로젝트 파일명
* war안에는 src, WEB\_INF폴더가 반드시 존재해야한다.

### JSP 컴파일

* 자바 소스 파일들은 src폴더안에 저장되고 WEB-INF하위의 classes폴더 안에 class로 컴파일된다.
* 하지만 JSP파일은 src폴더가 아닌 다른 폴더안에 저장되므로 위와 같은 경로에 class로 컴파일 되는 것이 아니라 WAS가 컴파일 해주는 것이다.

## local - Web - App

### Local

* Java\(화면, 통신, 스레드, 로직까지\)
* 동기화되지 않는다.
* thread와 socket을 직접 구현해 서버를 구축해야 하므로 시간과 비용이 든다.
* 자바에서도 http를 제공하는 클래스 API가 존재하기 때문에 웹 서비스를 제공할 수는 있다.
* a.class에서 b.class를 인스턴스화 해서 원본을 파라미터로 넘긴다.

### Web App

* **정적** : html, JS, CSS - 처리주체인 브라우저에는 객체가 내장되어 있어 이벤트 처리가 가능하다. - 하지만 비동기 - html은 재사용성이 떨어지고 유지보수가 불리하다. -&gt; xml
* **동적** : JSP, Servlet, Java - 비동기를 보완하기위한 언어 - 통신을 이용해 정보를 동기화한다. - JSP\(View\) - 디바이스마다 다른 설정을 제공하는 반응형 웹 - Java\(로직, 제어문\) - WAS제품이 스레드 관리를 해주므로 java로 직접 스레드를 구현, 관리할 필요가 없다.
* 페이지 이동이 빈번하게 일어나 URL이 바뀐다.
* main메서드가 없는대신 WAS가 제공해주는 req, res객체로 doGet메서드를 오버라이드한다.
* req, res객체로 듣기와 응답을 처리할 수 있다.
* 기능처리를 위해서는 순수한 java가 필요하다. = Modal 계층

### App\(Mobile - Android, IOS\)

* Android\(google\), IOS\(apple\)
* 모바일 스마트 서비스
* 안드로이드는 xml형식으로 화면을 구현한다.
* 하이브리드 방식
* 네이티브 방식

## JSP

### 학습목표

1. 페이지 이동방법에 대해서 말할 수 있다.
   1. location.href='xxx.jsp'
   2. url : 'xxx.do' - easyui-datagrid-이벤트발생시
   3. ajax\( { url : 'xxx.jsp', dataType : 'json', success:function\(data\){ } } \)
2. 페이지 이동 시 자료를  유지하는 방법에 대해 설명하고 활용할 수 있다.

### JSP

* JSP &lt; Servlet &lt; Java - JSP는 확장자가 jsp, Servlet은 확장자가 java - Java의 부모는 Object로 req, res를 지원하지 않아 직접 스레드를 구현, 관리 해야한다. - Servlet은 서버에게서 req, res를 객체주입 받아 스레드는 WAS가 관리해준다.
* mime타입에 따라 문서의 성격이 달라진다. - html, xml, json, ...등 - 서브타입이 html인 jsp문서라면 태그가 있더라도 html문서취급되어 내용만 보여진다.

### JSP와 HTML

| JSP -  text/html | HTML |
| :---: | :---: |
| &lt;% 자바소스코드 %&gt; | 자바코드 사용 불가 |

* JSP안에서는 자바코드를 최소화 해야한다. - 재사용성과 scope를 위해 - request.sendRedirect = page scope, foward = request scope

{% page-ref page="../untitled-20/" %}

### JSP에 자바코드 최소화하기

* JSP안에는 가급적 자바코드를 적게 사용한다.
* 자원관리는 직접하지 않는다. - 동시접속자가 많을수록 많은 스레드가 필요하고, 이 스레드들이 바라보는 객체\(JSP\)는 원본 하나다. - 자원관리를 싱글톤으로 이뤄져야한다. - 하나를 나눠써야 하므로 대기가 일어날 수 있다.

### &lt;% 자바소스코드 %&gt;

* 스크립틀릿
* 자바코드를 작성할 수 있다.
* 변수를 선언할 수 있지만 지역변수만 가능하다.
* 메서드 선언은 불가능
* 제어문은 사용할 수 있다.
* document.write와 같은 역할을 할 수 있다. - out.print\( \); - String test;와 String test=null;은 다르다. 전자는 출력하면 500번이 뜨고, 후자는 null이 출력된다.
* &lt;% out.pritn\( html코드 \); %&gt;
* 하지만 JS변수에 대입할 경우에는 정적이 되어버린다. 처음에 결정된 값을 가지므로

### scope

{% page-ref page="../untitled-1/" %}

### JSP -Java간 인스턴스, scope

## 페이지이동 : JSP와 Servlet

### 페이지이동과 session id

* 페이지 이동이 일어나면 sessiond id가 새로 만들어지고, 이 session id는 cookie에 저장된다.
* session id는 해당 접속을 식별하는 값이다.

### 모델1\(기본\) : JSP - JSP

* 모델1 = JSP로 요청을 받았다.
* 요청 -&gt; JSP\(로그인\)----- 이동 -----&gt; JSP\(처리\) -&gt; 응답
* 요청 : URL로 요청한다. - Web서비스를 이용하는 브라우저가 처리주체이므로 url을 통한 요청을 한다.
* JSP\(로그인\) : html역할로서의 JSP
* 이동 : request.sendRedirect\( \)  - 이 메서드를 만나면 연결을 끊고 새 JSP페이지와 새 연결을 한다.
* 클래스 이름을 알 수 없어 인스턴스화 할 수 없다.
* 객체\(JSP\) 자원관리를 시스템\(WAS\)이 싱글톤으로 해준다. 화면상으로 볼 수 없다. - 개발자가 관리하는 것보다 일관성 있고 안전성이 높다.

### 모델2 : Servlet - JSP

* Servlet\(java\)로 요청을 받는다. - java이지만 main메서드가 없다. 브라우저에서 실행되므로 필요없다. - 그래서 URL이 필요하다.
* java에서 JSP를 부를수 있게, 이동할 수 있게 해주는 것 - sendRedirect\( \); - Servlet과 JSP, JSP와 JSP간에 이동할수  있다. - 하지만 scope를 사용할 수 없다.

### 페이지 이동 메서드

## 반응형 웹 - 뷰포트

### 반응형 

* 디바이스\(장치\)의 해상도에 따라 다르게 대응되는 웹을 반응형 웹이라 한다. - 컴퓨터, 태블릿, 노트북, 핸드폰 등 디바이스 마다 해상도가 다르다.
* PC화면을 핸드폰에 최적화 하지않고 사용한다면 핸드폰으로 해당 사이트를 살펴보기에 힘들것이다.
* PC와 모바일 페이지를 별도로 제작해 사용한다면, 관리자 입장에서는 새 페이지를 생성, 관리 하는 것이므로 비효율적이라 볼 수 있다.
* 이러한 문제를 해결하기위한 반응형웹을 만들때  viewport가 사용된다.

### viewport 지정

* &lt;head&gt;영역에서 작성한다.
* &lt;meta name="viewport" content="속성=값", .... /&gt;

### viewport content 속성

| 속성 | 설 |  |
| :---: | :---: | :---: |
| width | 뷰포트 너비 | device-width |
| height | 뷰포트 높이 | device-height |
| user-scalable | 확대/축소 가능 여부 | yes\(기본값\) or no |
| initial-scale | 초기 확대/축소 값 | 1\(기본값\)~10 |
| minimum-scale | 최소 확대/축소 값 | 0~10 \(기본값 0.25\) |
| maximum-scale | 최대 확대/축소 값 | 0~10 \(기본값 1.6\) |

### 화면 크기에 따라 색상 지정

```markup
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<!-- 디바이스에 따라 화면출력이 다르게 이루어진다. -->
<meta name="viewport" content="width=device-width, initial-scale=1. minumum-scale=1, maximum=1, user-scalable=no">
<title>미디어 쿼리 사용하기</title>
<style type="text/css">
	@media all and (min-width:320px){/*브라우저 크기가 320이상이면 pink로 변경한다.*/
		body{
			background: pink;
		}
	}
	@media all and (min-width:668px){/*브라우저 크기가 600이상이면 pink로 변경한다.*/
		body{
			background: gray;
		}
	}
	@media all and (min-width:800px){
		body{
			background: #6fc0d1;
		}
	}
</style>
</head>
<body>
</body>
</html>
```

* 디바이스 화면 너비가 320px 이하 : 흰색
* 디바이스 화면 너비가 320px 초과 668px이하 : 분홍색
* 디바이스 화면 너비가 668px 초과 800px이하 : 회색
* 디바이스 화면 너비가 800px 초과 : \#6fc0d1 색상

후기 : 

