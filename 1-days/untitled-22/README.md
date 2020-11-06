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
* myBatis는 xml파일하나에 모두 관리한다. 

{% page-ref page="java-mybatis-db.md" %}

### MyBatis : SqlSessionFactory와 XML

* SqlSessionFactory는 XML문서\(Configuration.xml\)의 연결정보를 수집해 연결통로를 생성한다.
* 이 떄 Configuration.xml에는 사용될 sql문 모음 xml이 등록, 매핑되어 있어야한다. - xml 매핑시 xml문서의 이름은 업무 이름으로 하는것이 구분하기 좋을 것이다.
*  sql문을 저장한 xml문서는 수정되어도 서버를 다시 시작하지 않아도 반영된다. - xml문서가 프로젝트 안에서 java -&gt; class로 컴파일 되어 저장되기 때문이다. - 프로젝트의 src파일 안에서 소스로 관리되기 때문 - 프로젝트 - src내부의 문서들은 새로 저장될때 모든 소스가 컴파일 된다.
* 하지만 web.xml같은 배치서술자 파일은 src밖에서 관리 되므로 재시작해야한다. - 프로젝트 - WebContet 파일 안에서 관리 되므로 컴파일 되지 않는다.

### Query문

* Java : "Select empno, ename, ... From emp Where empno = ? " - VO의 getter, setter를 사용한다. - 코드가 길어진다. - 메서드안에 분산되어 있어 업무를 전체적으로 파악하고 sql문을 찾기에 불편하다. - sql문을 Eclipse, java에서 관리하므로 orcle이 인식하도록 " "이 붙는 것인데, Toad에서 단위 테스트를 하기 위해서는 " ", append를 떼어내 가져가야하므로 번거롭다.
* MyBatis - sql문에 " "를 사용하지 않아도 된다.  - toad에서 단위테스트시 그냥 복사해서 사용할 수 있다. - xml문서안에 해당 업무에 관련된 모든 sql문을 관리하므로 파악하고, 찾기에 유리하다.

### Java 배포

* WEB-INF - lib : jar모음 - classes : class모음
* 파일을 배포할 떄에는 war형식으로 묶에 배포한다. - WEB-INF폴더 안의 classes파일들을 war로 묶는다.
* 배포되는 war파일안에는 class파일만 존재해야한다.
* java파일안에는 주석들이 달려있어 java를 배포하는 경우에는 보안의 문제가 발생할 수 있다.
* war형식의 파일은 배포하기만 하면 압출풀기와 같은 과정 없이 사용할 수 있다.

