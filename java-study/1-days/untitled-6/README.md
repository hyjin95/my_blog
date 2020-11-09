---
description: 2020.10.13 - 40일차
---

# 40 Days - 예외처리, mybatis-DB연동,동적SQL, 싱글톤,XML DTD,스키마, xml설정파일, 스키마, DOM, ML

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261 : Oracle.com
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5
* 사용 FrameWork - MyBatis

## 복습

### mybatis의 역할

* 자바와 DB를 연결하는데 도움을 주는 프레임워크
* 자바의 DAO클래스 - mybatis Layer - Oracle - 연결 속지인 mybatis Layer를 사용하려면 mybatis.jar파일이 필요하다.
* mybatis의 XML파일은 두개가 필요하다.
* XML 1 = mapperConfig.xml - XML 1 : 오라클 연결에 필요한 정보를 담아 XML 2을 요청하는 XML - SqlSessionFactory, SqlSession클래스를 사용한다. - 연결을 처리하는 XML이므로 name, url 등의 객체를 생성해야한다.
* XML 2 = dept.xml, emp.xml - XML 2 : 요청될 SQL문을 담은 XML = dept.xml, emp.xml

### SqlSessionFactory 클래스

* 스프링에서는 SqlSessionFactoryBean 과 같다.
* DB와의 연결통로 역할
* commit, rollback이 가능하다.
* SqlSession  - sql처리 요청, 가져온 xml2을 오라클에게 요청, 말하기 한다.
* SqlSessionFactory클래스가 먼저 메모리에 저장되어야 SqlSession을 사용할 수 있다. - 서로 의존관계에 있다. - 의존관계 = 스프링 핵심 키워드

## 필기

### 예외처리

* error 1 : compile error - 문법에러 발생시 실행불가
* erroe 2 : runtime error - 문법에러가 아닌 논리오류, 실행 중에 에러가 발생한다.

### XML DTD, 스키마\(schema\)

* DTD, Schema는 html과 XML문서의 구조를 표현하기 위해 사용되는 파일이다.
* **DTD** 

  - 하나의 시스템 안에서 사용할 XML데이터의 구조를 정의해 유효성 검사시 사용한다.  
  - namespace, 상속개념을 지원하지 않는다.  
  - 하나의 문서에만 적용된다.  
  - 한정된 데이터 타입의 종류만 사용해야한다.  
  - DOM을 지원하지 않는다.  
  - 읽기만 가능한 파일로, 동적으로 구현할 수 없다.

* **Schema**

  - 서로  다른 시스템 사이에서 데이터를 주고받아 사용할수 있도록 데이터 표준화하기위해 사용한다.  
  - 문서의 구조와 데이터를 기술하기위해 구조\(데이터 타입 조합\)와 데이터 타입으로 나뉘어있다.  
  - namespace와 상속개념을 지원한다.  
  - 한정된 타입이 아닌 새로운 형식 정의가 가능하다.  
  - DOM을 통한 조작이 가능하다.  
  - 런타입시 상호작용하는 동적 스키마로 구현할 수 있다.

* 프로그램 방식에서 데이터 처리시 DTD나 스키마를 사용하면 안정성이 높아진다.

### DOM

* 문서 객체 모델\(The Document Object Model\), 웹 페이지의 객체 지향 표현
* html, xml문서의 프로그래밍 인터페이스로, html, xml문서들을 위한 구조를 제공한다. - **웹 페이지를 스크립트, 프로그래밍 언어에서 사용될 수 있게 연결시키는 역할** - 문서의 구조화된 표현을 제공해 프로그래밍언어가 DOM구조에 접근할수 있게 한다. - 프로그래밍 언어가 문서 구조, 스타일, 내용등을 변경할 수 있게 해주는 것 - 구조화된 nodes와 property, method를 갖는 Object로 문서를 표현해준다.

### ML\(HTML, XML\)

* Markup Languege
* 루트태그, 상속개념이 있어야한다.
* 태그안에 속성을 담을 수 있고, dtd파일안의 태그를 사용할 수 있다.
* 태그로 감싸\(마크\)서 사용하는 언어

### XML 문법검사

* Java의 comfile, Oracle의 parsing과 같은 개념
* 문법검사를  통과하면 DBMS의 옵티마이저가 dtd파일의 범위 내에서 실행계획을 세운다. - Cursor의 위치에 데이터 존재유무를 파악하고, 존재하면 패치, close한다.
* **패치** : 램에 로딩 후, 램에서 데이터를 가져오는 것

## MyBatis와 DB연동

### API

| JDBC API | MyBatis API |
| :---: | :---: |
| Connection | SqlSessionFactory |
| PreparedStatment | SqlSession |
| executeQuery | selectOne, selectList, slectMap |
| ResultSet | X |
| While | 자동 |

* 새로운 언어, 프레임워크 등을 사용하기 전에는 항상 API를 확인한다.

### MyBatis 설정파일 작성

* mybatis프레임워크가 참조하는 XML파일은 MyBatis설정파일과 SQL mapper파이롤 나뉜다.
* mybatis설정파일 - 자체 connetion pool을 구축 - 여러 DB연결정보 설정을 통해 용도에 따라 DB를 골라 사용할 수 있다. - select결과 캐싱 - VO\(값 객체\)에 alias 부여

### MyBatis 동적SQL

* 동적SQL = Dynamic SQL : **사용될 SQL문이 runtime시에 결정되는 SQL문**을 말한다.
* MyBaits프레임워크가 제공하는 동적SQL은 하나의 SQL문으로 여러 케이스르 처리할 수 있다. - ex\) 정렬조건에 따라 ORDER  BY절을 바꿔야하거나, 검색조건에 따라 WHERE절을 변경해야할때          동적 SQL기능을 이용하면 자동으로 변경되는 SQL문을 만들 수 있다.
* &lt;where&gt;의 값을 비교할 수 있다. - where : 조인검색시 조건값이  없으면 비교못하고 null을 반환하고, 조건 값이 존재해야 비교한다. - &lt;where&gt;if문&lt;/where&gt; 태그로 if문을 감싸면 where 1=1이런 조건 없이도 항상 if문이 진행된다.  - &lt;select id = ”findBlogWithTitleLike” parameterType=”Blog” resultType=”Blog”&gt;   SELECT \* FROM BLOG WHERE state = ‘ACTIVE’   &lt;if test="title!=null;&gt;   AND title like \#{title} &lt;/if&gt; &lt;/select&gt;
* [https://atoz-develop.tistory.com/entry/MyBatis-%EB%8F%99%EC%A0%81-SQL-choose%EC%99%80-set%EC%9D%84-%EC%82%AC%EC%9A%A9%ED%95%98%EC%97%AC-%EB%8F%99%EC%A0%81-SQL-%EB%A7%8C%EB%93%A4%EA%B8%B0](https://atoz-develop.tistory.com/entry/MyBatis-%EB%8F%99%EC%A0%81-SQL-choose%EC%99%80-set%EC%9D%84-%EC%82%AC%EC%9A%A9%ED%95%98%EC%97%AC-%EB%8F%99%EC%A0%81-SQL-%EB%A7%8C%EB%93%A4%EA%B8%B0)
* &lt;if&gt;, &lt;choose&gt;, &lt;where&gt;, &lt;trim&gt;, &lt;set&gt;, &lt;foreach&gt;, &lt;bind name&gt;

{% page-ref page="../16-days/" %}

## MyBatisZipCodeSearch

{% page-ref page="untitled/" %}

후기 : 프로젝트를 참여할까 아니면 혼자서 만들어볼까..고민이 깊은 날

