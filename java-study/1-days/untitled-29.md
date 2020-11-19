---
description: 2020.11.18 - 66일차
---

# 66 Days - DB:SQL-NoSQL, 스키마, 트랜잭션, connection pool, include:액션태그-다이렉티브, ajax:초성검색준비, 버퍼, flush

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5
* 사용 서버 - WAS : Tomcat

## 복습

### 자원

* 1단계 : 변수 - 1개 값
* 2단계 : 배열 - 같은 타입, n개 값
* 3단계 : 객체배열 - n개 타입, n개 값, 끼어들기 불가, 크기도 유동적이지 않다.
* 4단계 : List\(Array, Vector\), Map - 끼어들기 가능, 크기 유동적, 시간은 유지되지않는다.\(page scope\)

### scope

* page, request, session, application

### DML 반환값

* Select : cursor가 가르키는 row의 data - Resultset이 제공해주는 next\( \)함수로 커서를 이동할 수 있고, data가 여러개인 경우에는 while\(rs.next\( \)\) 를 사용한다.
* Update, Insert, Delete : 성공하면 1, 실패하면 0

### 오라클이 제공하는 Object

* 트리거, Procedure, function, Table, ....

### PK

* Primary Key
* equal join에 사용된다.
* index를 갖는다.
* Map의 key와 같이 unique한 값이다.

### index

* row에 index를 부여할 수 있다.
* 중복 index를 정렬할 수 있어 중복될 수 있다.
* index를 사용한 검색은 테이블 전체를 훑는것이 아니므로 속도가 빠르다.
* index 검색은 자동 정렬된다.

### List와 Map의 차이점

* Map은 값마다 key를 갖기때문에 List보다 직관적이라 할 수 있다.
* List는 들어오는 값을 정렬해서 갖지만 Map은 그냥 꽂기때문에 읽고 쓰는 속도가 Map이 빠르다.

## 필기

### DB : SQL

* 관계형 DB관리시스템\(RDBMS\)와 상호작용하는데에 사용되는 쿼리언어
* 데이터는 정해진 스키마에 따라 DB테이블에 저장된다. - 스키마가 명확하게 정해져 있어 데이터 무결성이 지켜진다.
* 데이터가 테이블 구조\(structure\)인 컬럼\(field\)와 로우\(record\)에 일치하지않으면 저장할 수 없다.
* 참고 : [https://siyoon210.tistory.com/130](https://siyoon210.tistory.com/130)

### DB:NoSQL

* MongoDB
* 프로시저, 사용자 정의함수, 트리거를 제공하지 않는다.
* table이 아닌 document에 데이터를 JSON이나 key-value형태로 저장하기 때문에 데이터를 읽어오는 속도가 빠르다.
* 스키마\(테이블 구조\)를 정의 하지 않아도된다.
* 데이터가 중복될 수 있다.
* 참고 : [https://siyoon210.tistory.com/130](https://siyoon210.tistory.com/130)

### 테이블, 레코드, 스키마

* Table : RDBMS에서 데이터를 저장하는 장소
* record : 하나의 컬럼데이터 모음\(row 한줄\), 한 테이블은 n개  record로 구성된다. - header\(컬럼's\), .....
* Schema : 테이블 구조 정보 - 컬럼, 컬럼 타입, 컬럼 크기  

## 트랜잭션\(작업단위 묶음처리\)

### 트랜잭션

* 테이블을 두개 이상 관리하는 경우에 많이 사용되는 처리 방식이다.
* 한 작업단위를 묶어서 처리하는데, 모든 요청의 sql이 성공하면 commit, 한 요청이라도 실패하거나 에러가 발생하면 rollback되는 처리이다.
* JDBC의 경우에는 기본적으로 자동 commit을 해주기 때문에 통신이 일어날때 자동커밋을 꺼야한다. - con.setAutoCommit\(false\); - 여기서부터 트랜잭션의 시작이라 할 수 있다.
* 자동커밋 종료\(트랜잭션 시작\) - 쿼리문 진행 - con.commit\(트랜잭션 종료\)
* 한 작업 단위마다 이 과정을 반복해야한다. - 한번에 처리해주는 프레임 워크 : Spring
* 한번에 처리해주는 프레임워크를 작성해보자\(게임엔진을 만드는 것과 비슷하다.\)

### 테이블 분리와 트랜잭션 사용 의의

* 예시로, 게시글과 첨부파일을 DB에 관리할때
* 테이블 하나에 관리하는 경우 - 첨부파일 컬럼갯수를 정해야 하므로 첨부파일 컬럼이 3개뿐이라면 사용자가 첨부파일을 4개 올리고 싶을때에는 게시글을 두개를 작성해야하는 일이 발생 할 수 있다. - 이런 일이 발생하면 안되기 때문에 게시글과 첨부파일의 테이블은 분리해 관리해야한다.
* 테이블 두개로 관리하는 경우 - 사용자가 첨부파일 업로드 요청을 하면, 게시글 테이블에도, 첨부파일 테이블에도 insert되어야 한다.  - 한 요청에 대한 작업을 같이 처리해야하는 것인데, 이때 필요한 것이 트랜잭션이다.
* 만일, 트랜잭션없이 하나 성공시 commit을 해버린다면 뒤의 작업이 실패해도 DB에 앞의 작업이 작성되기때문에 사용될수 없는 정보들이 DB에 쌓이게 될 수 있다. - 이를 방지해야한다.

### 트랜잭션 적용 위치

![&#xD2B8;&#xB79C;&#xC7AD;&#xC158;&#xCC98;&#xB9AC; &#xC608;&#xC2DC;](../../.gitbook/assets/1%20%2868%29.png)

* Controoler와 Logic의 사이에서 시작되어야 할 것이다.
* OrderController - OrderLogic - OrderDao : Order업무에 대한 수직관계 클래스
* memberLogic, GoodsLogic, OrderLogic : 업무 처리중 같은 단계에 있는 수평관계 클래스
* 예를들어, 회원이 상품을 주문하는 요청이 들어왔다면, 회원테이블에도, 상품테이블에도, 주문테이블에도 변화가 있어야 한다.  이렇게 한 요청에 대해 여러 업무의 Logic클래스들이 사용될때 트랜잭션처리를 한다.

## 커넥션 풀\(connection pool\)

### 커넥션 풀의 사용 의의

* 요청이 들어오면 DB에 접근해서 data를 꺼내고, 다시 가져오는 이 과정은 시간이 걸린다. - 버퍼링이 발생하는 이유
* DB connection Pool은 data를 미리 꺼내놓는 것 이다. - 가상 DOM과 비슷한 역할을 수행한다.
* 아파치에서 제공하는 connection pool을 사용해보자

### eclipse사용시 : 커넥션 풀 사용 준비

![connection pool&#xAD00;&#xB828; jar&#xD30C;&#xC77C;](../../.gitbook/assets/2%20%2852%29.png)

1. 필요한 jar파일을 프로젝트 WEB\_INF에 배포한다. - [http://commons.apache.org/proper/commons-dbcp/download\_dbcp.cgi](http://commons.apache.org/proper/commons-dbcp/download_dbcp.cgi)
2. 프로젝트에 해당 jar파일을 build Path에 추가한다.
3. 사용하는 서버.xml에 커넥션 풀에대한 코드를 작성해야한다. - &lt;Resource&gt;, &lt;ResourceLink&gt;
4. 배치서술자 파일에 등록
5. JSP로 테스트 구현 해보기

### 3. server.xml 작성1 : &lt;Resource&gt;

```markup
<GlobalNamingResources>
<Resource auth="Container" 
          driverClassName="oracle.jdbc.driver.OracleDriver" 
          maxActive="30" maxWait="1000" username="scott" password="tiger" 
          name="jdbc/dbPool" type="javax.sql.DataSource" 
          url="jdbc:oracle:thin:@192.168.0.187:1521:orcl11"/>
</GlobalNamingResources>
```

1. 인증 : 톰캣제공 컨테이너 사용 - auth="Container"
2. 드라이버 클래스 : 오라클제공 드라이버 사용 - driverClassName="oracle.jdbc.driver.OracleDriver"
3. 최대 활동 인원 - maxActive="30"
4. 최대 대기 시간 - maxWait="1000" : 1초
5. 계정정보 - username=" " password=" "
6. 원격 객체를 주입받을 수 있는 이름 - 톰캣 서버의 관리 감독을 받아 안전하다. - 자바의 인스턴스화는 자바에서만 사용가능하지만 이 이름은 어느 파일에서나 사용할 수 있다. - name="jdbc/dbPool"
7. 타입 : 커넥션 연결할때 사용 \(drivergetConnection\) - 타입 : 인터페이스, 추상클래스 - 데이터 소스 타입이 클래스보다 범위가 크다 - type="javax.sql.DataSource"
8. url : ip, port번호, ... - url="jdbc:oracle:thin:@ip:port번호:orcl11"

### 3. server.xml 작성1 : &lt;ResourceLink&gt;

```markup
<Host>
<Context docBase="dev_html" path="/" reloadable="true" source="org.eclipse.jst.jee.server:dev_html">
<ResourceLink global="jdbc/dbPool" name="jdbc/dbPool" type="javax.sql.DataSource"/>
</Context>
</Host>
```

* 적용할 프로젝트의 Context태그에 작성해야한다.

### 4. 배치서술자파일에 등록 : IoC

```markup
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd" id="WebApp_ID" version="4.0">
	<resource-ref>
		<description>Connection</description>
		<res-ref-name>jdbc/dbPool</res-ref-name>
		<res-type>javax.sql.DataSource</res-type>
		<res-auth>Container</res-auth>
	</resource-ref>
</web-app>
```

* web.xml에 connection pool을 등록하는 것은 외부에서 관리받기 위함이다. - 역제어\(loC : Inversion of Controll\)
* 서블릿을 배치서술자파일에 url을 등록함으로서 외부 WAS가 관리하게 하기 위한것과 같다.

### 5. JSP으로 테스트 구현 : jdbcPoolTest.jsp

![&#xCEE4;&#xB125;&#xC158; &#xD480; test](../../.gitbook/assets/4%20%2834%29.png)

```java
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ page import="java.sql.Connection, java.sql.PreparedStatement" %>
<%@ page import="java.sql.ResultSet, javax.naming.*, javax.sql.*" %>

jdbcPoolTest.jsp

<%
	Connection con = null;//물리적 거리가 잇는 오라클 서버 연결통로
	PreparedStatement pstmt = null;//쿼리문 요청 인터페이스
	ResultSet rs = null;//select시 커서 조작 인터페이스
	try{
		Context init = new InitialContext();
		DataSource ds = (DataSource)init.lookup("java:comp/env/jdbc/dbPool");
		String sql = "SELECT deptno, dname ,loc FROM dept";
		con = ds.getConnection();
		pstmt = con.prepareStatement(sql);
		rs = pstmt.executeQuery();
		while(rs.next()){
			out.print(rs.getInt("deptno")+", "+rs.getString(2));//2는 dname이지만 직관적이지 않으므로 "dname"을 사용하자
		}
	}catch(Exception e){
		e.printStackTrace();
	}
%>
```

## include : 액션태그와 다이렉티브

### include : 액션태그와 다이렉티브

![](../../.gitbook/assets/5%20%2824%29.png)

### 액션태그

* 액션태그 : &lt;namespace : 태그이름&gt; - &lt;jsp : include&gt;, &lt;jsp : forward&gt; - namespace를 사용한다는 것 : xml기반이다.
* 속성으로 page : url=" "와 flush : true \|\| false를 갖는다.
* 두 페이지가 main\_jsp.class와 sub\_jsp.class로 나뉘어 저장된다. __- &lt;jsp:param&gt; 태그를 사용해 변수를 파라미터로 가질 수 있다. - sub.jsp : getParameter\(" "\);

### 다이렉티브

* 다이렉티브 : &lt;% 태그이름 %&gt; - &lt;% include %&gt;
* 기존 페이지와 서브페이지의 결과로 만들어진 java문서가 하나의 class로 저장된다.

### 버퍼

* 클라이언트에게 화면을 내보내기 전 임시 저장소 개념
* JSP에서는 버퍼의 사이즈를 정할 수 있다.
* 지정 사이즈에 내용이 가득 차면 flush가 false이더라도 밀어낸다\(내보낸다\).

### include : flush

* include에서는 flush의 기본값은 false이다.
* **true**라면 - include구문을 만났을 때, 여태까지 처리된 main.jsp의 내용을 버퍼에서 클라이언트에게 내보내기\(버퍼 비우기\)하고, 다시 include로 이동된 페이지 sub.jsp의 내용을 담는다. - 일단 출력 버퍼의 내용을 웹 브라우저에 전송하게 되는데 이때 헤더 정보도 같이 전송된다.    일단 정보가 웹 브라우저에 전송이 되고 나면 해당부분에  정보를 추가해도 결과가 반영되지 않는다.
* **false**라면 - include구문을 만났을 때, 여태까지 처리된 main.jsp의 내용을 가지고 있다가 include로 이동된 페이지 sub.jsp의 내용의 결과까지 담기면 한번에 클라이언트에게 내보낸다.\(버퍼 비우기\)
* include의 경우라면 false를 많이 사용할 것이다. - 어차피 main.jsp페이지 안에 sub.jsp의 내용을 포함해서 내보내는 것이 목적이므로

### RequestDispatcher : include, forward

* include, forward 함수는 RequestDispatcher클래스가 제공한다.
* RequestDispatcher view = request.getRequestDispatcher\("View담당 html url"\); - getRequestDispatcher의 리턴값은 RequestDispatcher이다. - 그래서 싱글톤으로 관리되는 것이다.
* include와 forward는 request객체와 함께 사용된다.
* url이 변하지 않는다. - forward : 출력되는 화면은 기존페이지가 아닌 이동한 페이지가 담당한다.  - include : 이동한 페이지의 출력결과를 기존페이지가 포함해 보여준다.

### 객체생성 방법 두가지 : new, getXXX

* = new XXX : 객체 생성을 직접한다.
* = getXXX\( \) : 객체 생성 단계에서 null을 점검할 수 있고, 싱글톤으로 관리하는 방법이다. - getInstance\( \); - 메서드를 호출한 것이지만, 첫번째 방법인 인스턴스화와 효과는 같다.

### request, response의 역할

* request : 요청시, 이동시, 저장소\(sope\) - request가 값을 담아 서블릿에서 이동할 페이지는 view를 담당하는 html문서의 url이여야 한다.
* response : 응답시, 이동시\(response.sendRedirect\), mime type결정시, out객체 생성시 - response는 이동할 페이지가 서블릿이여도, jsp여도 된다. - 서블릿으로 이동하는 경우   JSP\(목록\) - Servlet\(요청1 : insert\) - Servlet\(요청2 : select\) - JSP\(응답출력\)

## ajax : 초성검색준비

### ajax : 조건

![&#xB124;&#xC774;&#xBC84; &#xC790;&#xC74C; &#xAC80;&#xC0C9; &#xAE30;&#xB2A5;](../../.gitbook/assets/3%20%2842%29.png)

* 네이버의 검색창을 참고한다.
* 자음에 해당하는 키보드를 눌렀다 떼면, 그때 조회 결과를 화면에 보여줘야한다.
* 밑에 출력되는 목록이 각자의 공간을 갖고있는 것을 볼 수 있다. 그러므로 인라인요소인 &lt;span&gt;태그가 아닌 블럭요소인 &lt;div&gt;태그를 사용한다.

### ajax : 코드

```javascript
$.ajax({ 
	 //속성이 와야하는 자리이므로 함수 호출 불가 
	 url : " "
	,dataType:html(화면)|JSON|xml
	,success:function(data){	}
});
```

* dataType으로는 JSON으로 꺼내와 table로 화면에 출력한다.
* success는 이벤트 핸들러이기때문에 함수를 사용할 수 있는데, 함수의 파라미터에는 url요청의 처리결과를 담을 수 있다.

후기 : 비대면 수업이 내일부터 이루어진다. 마지막 팀프로젝트를 위한 상담도 내일 내 차례이다. 계속 열심히 해서 팀프로젝트때 조에게 도움이 되었으면 좋겠다.

