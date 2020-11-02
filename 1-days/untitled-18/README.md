---
description: 2020.10.30 - 53일차
---

# 53 Days - dataset:xml-json, 페이지이동3, JSP-&gt;Servlet, nexacro grid,

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5 - Visual Studio Code
* 사용 서버 - WAS : Tomcat

## 필기

### 업무

* 디자인, 빅데이터, 모바일, 블록체인, 웹, ...

### 학습 키워드

1. **인스턴스화** - A a = new A\( \);   A a= null; a = new A\( \);
2. **객체주입** - 내부 여러 클래스 간의 관계에서 사용 - 어디에, 언제, 어떻게 사용할 것인지 - 테스트 - 배포를 위해 결합도를 낮춰야한다.
3. **의존성 주입** - 외부에서 import, link받아 사용 - jar : 클래스 묶음  - 서로 다른 시스템과의 관계 - 테스트 - 배포를 위해 결합도를 낮춰야한다.
4. **클래스 쪼개기** - 공통 코드 꺼내기, 역할에 따라 클래스 나누기 = 설계 - 단위테스트, 통합테스트 위주의 개발을 해야한다.
5. **화면과 로직 분리** - UI -&gt; 소통 - 듣기\(Parameter\) -&gt; 말하기\(return\) - return타입을 정할줄 알아야 한다. - 화면과 만나는 곳 :DataSet - DataSet : JAVA\(DTM\), APP\(Json, xml\) - 로직분리 = 인스턴스화, 객체주입, 의존성주입, 클래스 쪼개기
6. **MVC패턴** - 화면 : View, 로직: Model, Controller
7. **Spring F/W** - 인스턴스화, 객체주입, 의존성주입, 클래스 쪼개기를 이용해 개발자가 조립하던 것을 해준다. - 한 프레임워크가 조립해주므로 통일감있다.

### JSP -&gt; JAVA -&gt; Class

* xxx.jsp는 인스턴스화 할 수 없다.
* HTML에 자바코드를 추가하게 해주며, 문법검사를 하지 않으므로
* WAS서버 제품이 jsp파일이 실행될때 jsp-api.jar파일을 이용해 jsp파일을 xxx.jsp.java로 변환시킨다.
* 그 이후에 servlet-api.jar파일을 이용해 xxx.jsp.java를 xxx.class로 변환시킨다.

## 페이지이동3

### 페이지 이동 방법

1. **location.href = xxx.jsp - JS에서 사용**
2. **&lt;form action = xxx.jsp \|\| xxx.do&gt; - Tag에서 사용**
3. **$ajax\(data-options="url:xxx.do"\) - AJAX에서 사용**
4. java제공, sendRedirect\("URL"\) - JSP에서 사용
5. fetch

### 요청 처리 - jsp, servlet

* JSP가 요청을 받았을떄 - 구조 : 화면 - 로직 - 재사용성이 떨어진다. - 반복코드가 많아 일괄처리에 불리함
* Servlet이 요청을 받았을떄 - 

{% page-ref page="../untitled-14/" %}

{% page-ref page="../untitled-16/" %}

## nexacro - DatsSet

### 서비스 구성도

![](../../.gitbook/assets/.png%20%2811%29.png)

### html과 xml의 dataset

* UI솔루션 - xml기반 : 넥사크로 - JS기반\(HTML\) : easyUI, 부트스트랩
* easyui, 부트스트랩의 DataSet = JSON - 자바코드로 DataSet을 JSON형식으로 만들어준다.
* 넥사크로의 DataSet = xml - 자바코드로 DataSet을 xml형식으로 만들어준다.

## nexa의 Grid에 DataSet 담는 순서

### nexacro UI

* nexacro가 제공하는 grid의 dataSet에는 Object가 담길 것이다. - 그러므로 dataSet은 data의 타입을 갖고 있어야 한다.
* 그래서 제일 먼저 **header\(컬럼\)**을 정한다.

1. header에 타입을 정한다. \(컬럼타입\) -- 0번째, row는 1번째부터
2. JSP가 while\(rs.next\)를 사용해 커서가 true인 동안에 row에 data를 set한다.

### JSP조회 서비스 페이지 생성

1. Get방식으로 전송된 조회조건값을 받는다. getParameter
2. 조회 결과를 보낼 PlatformData를 생성
3. 데이터베이스 설정 정보를 작성
4. 조회된 결과 ResultSet 데이타를 Dataset 형태로 변환 - nexacro는 자체 DataSet타입으로 Java를 사용한다.
5. SQL을 작성하고 쿼리를 실행하여 처리 결과를 ResultSet 객체에 담는다. - while\(re.next\)로 커서가 true인 동안 data를 set한다.
6. Dataset을 PlatformData에 추가 -&gt; client화면으로 전송

## JSP\(xlml\)파일 Servelt파일로 변환

### nexa - xml \|\| Servlet 파일로 dataset 전송

* html\(화면\)은 처리주체가 브라우저이고, 브라우저는 URL로 파일을 호출한다. - main메서드는 사용하지 않는다. main메서드\(exe파일\)=local - 온라인 통신을 위해 http프로토콜을 사용한다. = URL
* 브라우저가 가장 먼저 스캔하는 것 = web.xml
* JSP는 URL을 갖고있어 URL로 바로 호출이 가능하다. Servlet은 URL을 갖고있지 않아 web.xml 배치서술자에서 url-pattern을 부여해줘 그 url로 접근한다.

### 브라우저의 web.xml스캔 순서

![&#xBE0C;&#xB77C;&#xC6B0;&#xC800;&#xC758; xml&#xC2A4;&#xCE94; &#xC21C;&#xC11C;](../../.gitbook/assets/4%20%2829%29.png)

### nexacro 제공

* PlatformData - 서블릿에서 처리된 결과를 담아 화면에 출력하는 클래스 - 넥사크로에서 이벤트를 감지하면 해당 요청에 대한 처리결과를 별도로 전달할 수 있는 클래스를 제공한다.
* VariableList - 서버단\(서블릿\)에서 처리한 결과에 따른 메세지를 전달할 때사용하는 클래스 - 서블릿에서 넥사크로 화면 페이지로 전달

### Servlet주의사항

* URL을 갖고있지 않다.
* get, post메서드만 가질 수 있다.
* WAS 서버가 라이프 사이클을 관리하므로 예외처리를 throws로 빼야한다.
* web.xml을 수정시 서버를 재가동해야 반영됨

### 순서

1. web.xml배치 서술자에 servlet파일 클래스명, url-pattern 매핑
2. java class로 superclass - HttpServlet을 상속받는 서블릿파일 선언 - 온라인 서버 제공을 위해 WAS의 req, res를 부여받는 Servlet에게서 객체를 주입받아 사용한다.
3. &lt;%@ page import %&gt;를 import한다. - ctrl + shift + o
4. API를 가져다 활용한다.

{% page-ref page="nexa-xml-dataset.md" %}

후기 : 삼전 주식 샀더니 떨어졋다...지금 팔건 아니지만...ㅠㅠ

