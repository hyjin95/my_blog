---
description: 2020.11.06 - 58일차
---

# 58 Days -

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5
* 사용 서버 - WAS : Tomcat

## Java와 MyBatis, xml

### Java와 MyBatis

<table>
  <thead>
    <tr>
      <th style="text-align:left"></th>
      <th style="text-align:center">JAVA</th>
      <th style="text-align:center">MyBatis</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">&#xC5F0;&#xB3D9;&#xC815;&#xBCF4;</td>
      <td style="text-align:center">ip, port, &#xACC4;&#xC815;&#xC815;&#xBCF4;</td>
      <td style="text-align:center">ip, port, &#xACC4;&#xC815;&#xC815;&#xBCF4;</td>
    </tr>
    <tr>
      <td style="text-align:left">&#xD30C;&#xC77C;</td>
      <td style="text-align:center">DBConnectionMgr.java</td>
      <td style="text-align:center">Configuration.XML</td>
    </tr>
    <tr>
      <td style="text-align:left">&#xC5F0;&#xACB0;&#xD1B5;&#xB85C;</td>
      <td style="text-align:center">
        <p>Connection( I )</p>
        <p>&#xAC1C;&#xBC1C;&#xC790;&#xAC00; &#xC9C1;&#xC811; &#xAC00;&#xC838;&#xC640;
          &#xC0AC;&#xC6A9;&#xD574;&#xC57C;&#xD55C;&#xB2E4;.</p>
      </td>
      <td style="text-align:center">
        <p>X</p>
        <p>myBatis&#xAC00; myBatis.jar&#xC758; class&#xB85C; &#xD574;&#xC900;&#xB2E4;.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Query&#xC804;&#xC1A1;, &#xC694;&#xCCAD;</td>
      <td style="text-align:center">PreparedStatment, Statment</td>
      <td style="text-align:center">X</td>
    </tr>
    <tr>
      <td style="text-align:left">Query&#xBB38; &#xAD00;&#xB9AC;</td>
      <td style="text-align:center">
        <p>String</p>
        <p>&#xCEF4;&#xD30C;&#xC77C;O</p>
        <p>&#xC5F0;&#xACB0; &#xC815;&#xBCF4;&#xAC00; &#xC720;&#xC9C0;&#xB418;&#xC9C0;&#xC54A;&#xB294;&#xB2E4;.</p>
      </td>
      <td style="text-align:center">
        <p>xml</p>
        <p>&#xCEF4;&#xD30C;&#xC77C;X</p>
        <p>&#xC5F0;&#xACB0; &#xC815;&#xBCF4;&#xAC00; &#xC720;&#xC9C0;&#xB41C;&#xB2E4;.</p>
      </td>
    </tr>
  </tbody>
</table>

* MyBatis를 사용하면 myBatis가 자신의 jar파일 내부 클래스를 사용해 해주는 것이 많다. - 코드가 줄어든다.
* java는 연결통로와 Sql문 전송 객체를 외부에서 개발자가 직접 가져와 사용해줘야 하지만, MyBatis는 자기가 가진 jar파일 내부의 class들을 사용해 외부에서 끌어오지 않아도 된다.
* java는 Query문을 String으로 관리하므로 sql문에 \( apppen, "  "\)를 사용하므로 번거롭다. - 또, java는 Query문을 메서드로 관리해 분산되어 있다.
* myBatis는 xml파일하나에 모두 관리한다.  - 업무별로 관리해 A업무xml에는 A업무의 모든 sql문이 관리되어 편하다.

{% page-ref page="java-mybatis-db.md" %}

### MyBatis : SqlSessionFactory와 XML

* SqlSessionFactory는 XML문서\(Configuration.xml\)의 연결정보를 수집해 연결통로를 생성한다.
* 이 떄 Configuration.xml에는 사용될 sql문 모음 xml이 등록, 매핑되어 있어야한다. - xml 매핑시 xml문서의 이름은 업무 이름으로 하는것이 구분하기 좋을 것이다.
*  sql문을 저장한 xml문서는 수정되어도 서버를 다시 시작하지 않아도 반영된다. - xml문서가 프로젝트 안에서 java -&gt; class로 컴파일 되어 저장되기 때문이다. - 프로젝트의 src파일 안에서 소스로 관리되기 때문 - 프로젝트 - src내부의 문서들은 새로 저장될때 모든 소스가 컴파일 된다.
* 하지만 web.xml같은 배치서술자 파일은 src밖에서 관리 되므로 재시작해야한다. - 프로젝트 - WebContet 파일 안에서 관리 되므로 컴파일 되지 않는다.
* MyBatis에서 연결통로를 생성할떄, XML문서로 따로 분리한 연결 담당 파일을 가져와 읽어야한다. - 연결을 담당하는 XML파일이 따로 존재하기 떄문에 해당 파일로 재사용이 가능하다.

### Query문

* Java : "Select empno, ename, ... From emp Where empno = ? " - VO의 getter, setter를 사용한다. - 코드가 길어진다. - 메서드안에 분산되어 있어 업무를 전체적으로 파악하고 sql문을 찾기에 불편하다. - sql문을 Eclipse, java에서 관리하므로 orcle이 인식하도록 " "이 붙는 것인데, Toad에서 단위 테스트를 하기 위해서는 " ", append를 떼어내 가져가야하므로 번거롭다.
* MyBatis - sql문에 " "를 사용하지 않아도 된다.  - toad에서 단위테스트시 그냥 복사해서 사용할 수 있다. - xml문서안에 해당 업무에 관련된 모든 sql문을 관리하므로 파악하고, 찾기에 유리하다.

### Java 배포

* WEB-INF - lib : jar가 저장되는 곳 - classes : class가 저장되는 곳
* 운영서버에 파일을 배포할 떄에는 war형식으로 묶어 배포한다. - WEB-INF폴더 안의 classes파일들을 war로 묶는다.
* 배포되는 war파일안에는 class파일만 존재해야한다.
* java파일안에는 주석들이 달려있어 java를 배포하는 경우에는 보안의 문제가 발생할 수 있다.
* war형식의 파일은 배포하기만 하면 압출풀기와 같은 과정 없이 사용할 수 있다.

### Eclipse War배포하기

![&#xC2DC;&#xC791;&#xD45C;&#xC2DC;&#xC904;&#xC5D0;&#xC11C; Configure&#xC11C;&#xBC84; &#xC2E4;&#xD589;](../../.gitbook/assets/1%20%2860%29.png)

![start&#xB97C; &#xB204;&#xB974;&#xBA74; &#xC11C;&#xBC84;&#xAE30;&#xB3D9;&#xC2DC; war&#xAC00; &#xC54C;&#xC544;&#xC11C; &#xD504;&#xB85C;&#xC81D;&#xD2B8; &#xBA85; &#xD30C;&#xC77C;&#xB85C; &#xC0DD;&#xC131;&#xB418;&#xB294; &#xAC83;&#xC744; &#xBCFC; &#xC218; &#xC788;&#xB2E4;.](../../.gitbook/assets/2%20%2847%29.png)

* 배포하기 - 프로젝트 우클릭 - Export - War.file - Destination:배포서버지정 - 서버파일의 webapps폴더 안에 저장 - Finish
* 배포한 서버의 Configure창에서 서버를 기동시키면 배포했던 War파일이 프로젝트명의 폴더로 생성되는 것을 볼 수 있다. - 기동한 서버는 Stop을 눌러 정지 시켜주자, Eclipse에서 사용할 수 있도록

### commit, rollback

* SqlSession으로 생성한 인스턴스 변수는 commit과 rollback의 대상이 된다. - SqlSession session = null; - 여기서 session은 서버 캐시메모리에 관리되는 API가 아닌 SqlSession의 인스턴스변수명일 뿐이다.
* 해당 인스턴스 변수로  insert\("empInsert\), update\("empUpdate",pmap\),delete\("empDelete",pmap\), selectOne\("empDetail"\), selectList\("empList"\), selectMap\("empMap"\) 같은 메서드를 호출 할 수 있다.
* 해당 인스턴스 변수로 자바단에 오라클 명령어 commit과 rollback을 호출할 수 있는데, - 자바 : con.setAutoCommite\(true\)을 사용해 자동커밋을 할 수 있다. - MyBatis : 별도로 commit, rollback을 인스턴스변수에 호출해주어야 한다.   con.commit\( \), con.rollback\( \), session.commit\( \), session.rollback\( \)
* 위 자바의 오토커밋, myBatis의 commit, rollback명령어들은 트랜잭션처리의 대상이 될 수 있다.

### MyBatis - 트랜잭션\(transaction\)

* 한번에 수행되어야 할 쿼리의 묶음\(Set\)
* 모두 다 정상적으로 수행되지 못한다면, 아무것도 수행되지 않도록 하는 묶음 단위 - 처리결과가 둘다 1이면 commit이고, 하나라도 0이면 rollback한다.
* 업무 복잡도가 높을 수록 사용 빈도가 많아진다.
* ex\) - 테이블1 : 주문M : 누가, 언제, 주문번호\(pk\)    테이블2 : 주문상세내역서 : 상품, 가격, 주문코드+식별번호\(복합키\)    회원이 주문시 두 테이블 모두에게 정보가 들어가야한다.

### JSP - 공백제거 &lt;trim&gt;

```markup
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd" id="WebApp_ID" version="4.0">
	<jsp-config>
		<jsp-property-group>
			<url-pattern>*.jsp</url-pattern>
			<trim-directive-whitespaces>true</trim-directive-whitespaces>
		</jsp-property-group>
	</jsp-config>
</web-app>
```

* JSP파일에서 JSON.parse\( data \); 로 parse를 요청하면 JSON내부의 parser가 문법검사를하는데,  이때 문서 형식 안에 공백\(whitespace\)가 존재하면 형식에 맞지 않다고 판단하고 진행되지 않는 경우가 발생 할 수 있다. Web.xml 배치 서술자에서 jsp형식의 모든 파일 그룹에 적용할 수 있다.

## HTML과 JSP

### HTML

```markup
<html>
    <body>
        안녕
    </body>
<html>
```

* 처리주체 : 브라우저

### JSP

```markup
<%@page contentType"text/html" %>
<html>
    <body>
        <% out.ptin("안녕"); %>
    </body>
<html>
```

* 처리주체 : 서버
* mimetype html이기때문에 서버가 문서를 읽을때 4번을 알아서 읽어준다. 화면에 출력되는 것은 안녕 뿐

### HTML과 JSP

```markup
<%@page contentType"text/html" %>
<html>
    <body>
        반가워요<br>
        <% out.ptin("안녕"); %>
    </body>
<html>
```

* 처리주체 : 서버 - &gt; 브라우저
* 서버쪽이 먼저 처리되므로 화면에는 안녕 이 먼저, 그 다음에 반가워요가 출력된다.

### JSP변수와 JS

```markup
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>a2.jsp</title>
</head>
<body>
<script type="text/javascript">
   var mem_id = "<%="test"%>";
   alert("mem_id : "+mem_id);
</script>
</body>
</html>
```

* mimetype이 html인 jsp문서
* JS변수 mem\_id에 값으로 test 라는 문자가 담겼다.
* JS에서 문자에는 ' '이나 " "를 붙이지 않으면 변수 취급하므로 문자열이라면 반드시 붙이도록한다.
* JS변수에 java변수를 담을 수 있다. 하지만 정적이다. 서버에서 이미 결정된 정보가 담기는 것이므로

후기 : 

