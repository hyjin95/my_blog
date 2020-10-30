---
description: 2020.10.30 - 53일차
---

# 53 Days - dataset:xml-json,

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5 - Visual Studio Code
* 사용 서버 - WAS : Tomcat

## 복습

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

### nexa - xml \|\| Servlet 파일로 dataset 전송

* html\(화면\)은 처리주체가 브라우저이고, 브라우저는 URL로 파일을 호출한다. - main메서드는 사용하지 않는다. main메서드=local
* 브라우저가 가장 먼저 스캔하는 것 = web.xml
* JSP는 URL을 갖고있어 URL로 바로 호출이 가능하다. Servlet은 URL을 갖고있지 않아 web.xml 배치서술자에서 url-pattern을 부여해줘 그 url로 접근한다.

{% page-ref page="nexa-xml-dataset.md" %}

후기 : 

