---
description: 2020.10.08 - 38일차
---

# 38 Days - MyBatis, XML, jar, VO와Map

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261 : Oracle.com
* 사용Tool  - Eclipse : Eclipse.org
* 사용 FrameWork - MyBatis

## 복습

### DML

* 데이터 조작어
* 오라클의 세그먼트라는 메모리 영역을 사용해 속도가 느리닫.
* SELECT, DELETE, INSERT, UPDATE
* SELECT  - 모든 업무의 시작이자 커밋, 롤백의 대상이 될 수 없다. - 조회 기능으로 데이터, 테이블에 변화를 줄 수 없다. - 반환값 = 커서 - executrQuery\( \) 의 반환값 ResultSet
* DIU의 반환값 = int타입 - x건이 성공적으로 삭제/입력/수정 되었습니다.
* ResultSet : 자바에서 오라클 커서를 조작하기 위해 제공되는 인터페이스

### DDL

* Object생성, 오라클 구조 정의어
* CREATE, DROP, .....
* 따로 메모리 영역을 사용하지 않아 처리 속도가 빠르다.

### DCL

* 권한을 부여해주는 언어
* GRANT, ....

### 쿼리문 작성시 오라클에게 요청시 사용하는 클래스

* JAVA JDBC API에서 제공되는 PreparedStatement

### JOIN

* DB에서 테이블을 보여줄때, 총계, 소계 등을 조건으로 한 테이블을 출력해야할때 사용된다.
* 카타시안 곱 : 묻지마 조인 - equal조인은 양쪽 테이블에 모두 존재하는 데이터를 검색할때
* outter 조인 - 양쪽테이블 중 한쪽에는 존재하지만 다른 쪽에는 존재하지 않더라도   양쪽 중 어느 한쪽이라도 존재하면 검색해준다.

## mybatis & mySQL

### 학습목표

* 여러 프레임워크를 만나도 활용할 줄 알아야한다.
* Oracle대신 mybatis 프레임워크를 이용해 db와 연동해보자

### mybatis 사용하기

1. 구글 - maven mybaris검색
2. mybatis 사이트 - 설치
3. 자바가상머신 폴더의 lib폴더안에 mybatis.jar파일 넣기 - jar파일이 없이 클래스를 사용하면 ClassNotFoundException이 발생한다.
4. Eclipse - 프로젝트파일 우클릭 - build path - configure build path - labraries  - add external JARs - 3번 jar파일 add
5. 가이드 확인하기 - mybatis 페이지 : [https://mybatis.org/mybatis-3/ko/index.html](https://mybatis.org/mybatis-3/ko/index.html)  - maven mybatis페이지 : [https://mvnrepository.com/artifact/org.mybatis/mybatis](https://mvnrepository.com/artifact/org.mybatis/mybatis)

{% file src="../../.gitbook/assets/mybatis-3-user-guide\_ko.pdf" caption="mybatis-user-guide" %}

### mySQL 사용하기

1. 구글 - maven mybaris검색
2. mybatis사이트에서 mySQL검색 - jar파일 설치
3. 자바가상머신 폴더 lib폴더안에 jar파일 넣기
4. Eclipse - 프로젝트파일 우클릭 - build path - configure build path - labraries  - add external JARs - 3번 jar파일 add

### 드라이버 클래스

* DB를 연동하기 위해서는 사용할 제품의 드라이버 클래스가 필수이다.
* Oracle은 oracle의 드라이버 클래스를, mySQL은 mySQL의 드라이버 클래스가 필요하다.
* class for\("드라이버 클래스 이름"\);

## 필기

### 첫 업무시 확인할 것

* oracle 등 db제품의 ip주소
* tomcat등 서버 버전정보, port번호

### Framework

* 개발할때 설계의 기본이 되는 뼈대나 구조, 환경
* 재사용, 확장이 가능한 라이브러리

### 영속성\(Persistence\)

* 데이터를 생성한 프로그램이 종료되더라도 사라지지 않는 데이터의 특성을 말한다.
* 영속성이 없는 데이터는 메모리에서만 존재하므로 프로그램을 종료하면 모두 사라진다.
* Object Peristence\(영구적인 객체\) - 메모리상의 데이터를 파일시스템, 관계형 DB또는 객체DB등을 활용해 영구적으로 저장한다.
* 데이터를 DB에 저장하는 방법 1. JDBC - java에서 사용 2. Spring JDBC - ex\)JdbcTemplate 3. Persistence Framework - ex\) mybatis

### ORM솔루션

* Object Raletional Mapping, 객체-관계 매핑
* 객체와 관계형 DB 데이터를 자동으로 매핑해주는 것. - 객체지향 프로그래밍은 클래스를, 관계형 데이터베이스는 테이블을 사용한다. - ORM을 통해 객체간의 관계를 바탕으로 SQL을 자동생성해 불일치를 해결한다.
* 소스의 양이 줄어들고, 프로시저 등을 지원한다.
* SELECT문 안에 if문을 사용할수 있게 지원한다.
* ORM솔루션은 순수하지 않다. - 인터페이스와 추상클래스를 제공받아 사용하는 프레임워크들 이기때문에 강제로 메서드 오버라이딩 발생한다.
* 장점 - 객체지향 코드로 더 직관적이고 각 객체에 대한 코드를 별도작성하여 가독성을 올려준다. - 선언문, 할당, 종료같은 부수적인 코드가 줄어들거나 사라진다. - 재사용, 유지보수에 편리하다. - DBMS에 대한 종속성이 떨어진다.    SQL을 자동생성하므로 RDBMS데이터 구조와 Java의 객체지향 모델 사이의 간격을 좁힌다.
* 단점 - ORM으로만 서비스를 구현하기는 힘들다. - 프로젝트가 복잡해질수록 잘못 구현되면 속도저하, 일관성이 무너질 수 있다. - 프로시저가 많은 시스템에서는 객체지향적인 장점을 활용하기 어렵다.   프로시저를 다시 객체로 바꿔야하므로

### Spring

* 자바 플랫폼을 위한 오픈소스 애플리케이션 프레임워크
* 동적 웹사이트를 개발하기위한 서비스를 제공하며, 순수한 프로그램은 아니다.
* 경량 컨테이너로 자바 객체를 직접 관리한다. - 애플리케이션 객체의 생명주기와 설정을 포함, 관리한다는 설정에서 일종의 컨테이너라 할 수 있다.
* 제어 역행\(Ioc, Inversion of Control\) : 애플리케이션의 느슨한 결합을 도모한다. - 컨트롤 제어권이 사용자가 아닌 프레임워크에 있어 필요에 따라 스프링에서 사용자의 코드를 호출한다.
* 의존성 주입\(DI, Dependency Injection\) : - 각 계층이나 서비스들 간의 의존성이 존재하면 스프링 프레임워크가 서로 연결시켜준다.
* 관점지향 프로그래밍\(AOP, Aspect-Oriented Programming\) - 여러모듈에서 **공통적으로 사용하는 기능을 분리하여 관리**가 가능하다.
* 배치 프레임워크 - 특정 시간대에 실행하거나 대용량의 자료를 처리하는데 쓰이는 일괄처리를 지원하는 배치프레임워크를 지원한다.

### VO & Map의 공통점

* row가 존재한다,. index값과 key를 통한 검색이 가능한 자료구조이다.

## mybatis

### mybatis

* 객체지향 언어 자바의 DB프로그래밍을 도와주는 개발 프레임워크
* 개발자가 작성한 **SQL문과 자바객체를 매핑**해주는 기능을 제공한다.
* **SQL명령어를 자바 코드에서 분리해 XML파일에 따로 관리**하는 특징을 갖는다. - 기존 Dao의 안에서 SQL문을 따로 분리한다.
* 개발자가 지정한 SQL저장프로시저 = 스토어프로시저
* JDBC코드와 수동으로 셋팅하는 파라미터와 결과 매핑을 제거한다.

   while\(rs.next\(0\){  
       DeptVO dvo = new DeptVO\( \);  
       dvo.serDeptno\(rs.getInt\(1\)\);  }   
      위와 같이 수동으로 값을 꺼내오는 작업이 생략가능하다. 해준다.

* 자바 POJO를 설정하고 매핑하기위해 XML과 애노테이션을 사용할 수 있다.

### mybatis 제공

* sqlSessionFactory 클래스 : 서버와의 연결통로
* sqlSession : 조회\(SELECT\): 리턴타입\(1row \|\| 1값\(Object\) \|\| n개 row\)
* Reader : 문서를 읽는데 필요한 클래스
* **sqlSession.selectOne\( \)** - 오직 하나의 객체, Object만을 리턴하는 메서드 - 1개 row : selectOne\( \) : XXXVO - 1개 값    : selectOne\( \) : Object\(int, integer, String, XXXVO\)
* **sqlSession.selectList\( \)** - n개의 객체가 리턴될때 사용하는 메서드 - n개 row : selectList\( \) : Map - 반환값은 Map타입이지만 n개의 Map타입 값이 나올수 있으므로 List에 담는다. 

### sqlSessionFactory

* 물리적으로 떨어져있는 오라클 서버와 연결통로를 확보하는데 필요한 객체 - 연결통로로서 sqlSession객체 생성에 대해 필요한 사전처리
* sqlSession은 위에서 연결되었을 때 개발자가 작성한 sql문 처리 요청을 하는데 필요한 객체 - sqlSession.commit\( \), sqlSession.rollback\( \)이 가능하다. - session변수.openSession\( \)메서드로 생성가능 - session변수.close\( \) 메서드로 닫아주어야한다.
* sqlSessionFactory가 먼저 메모리에 로딩 되어야\(인스턴스화 되어야\) selSession이 생성될 수 있다. - 서로 의존관계에 있다.
* build클래스로 sqlSessionFactory객체를 인스턴스화하여 사용한다. - SqlSessionFactory 변수이름 = new SqlSessionFactoryBuilder\( \).build\(Reader \|\| Writer\);

### 수동&자동

* 원시적인 방법으로 처리하는 것은 자동화 시스템에서 지원하지 않는 부분까지도 더 자세히 접근이 가능하므로 문제해결에 도움을 준다.
* 수동 : java JDBC API를 활용한 처리
* 자동 : 구글 mybatis가 처리

### POJO\(자바객체\)

* Plain Old Java Object
* 순수하게 자바로만 설계된 엔진
* 자바빈즈\(Javabeans\)객체

### XML

* mybatis프레임워크를 사용할때 사용하게되는 파일타입
* Eclipse에서 패키지 우클릭 -&gt; new -&gt; other -&gt; XML -&gt; XML File -&gt; 이름지정 -&gt; XML파일 생성
* SQL문을 한번에 볼수 있게되며, 각 **sql마다 id를 부여해 구분, 호출**할 수 있도록한다.
* 다른 두 업체가 다른 DB제품을 사용하거나 할때 사용하기 용이하다.
* 자바에서 따로 관리하는 장점 - 컴파일하지 않아도된다. - 한번 읽으면 끝까지 기억한다. - 버전관리가 용이하다. - 자바 코드와 분리된다.
* XML로 관리시 드라이버클래스, ip, port번호가 필요하다.
* 주석 : ctrl + shift + /

### XML 기본형태

```markup
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="oracle.mybatis.DeptMapper">
 <select id="getDeptList" parameterType="int" resultType="Map">
   select deptno, dname, loc from dept
 </select>
</mapper>
```

1. SQL mapper파일은 XML타입이므로 XML선언이 온다.
2. 태그 규칙을 정의한 DTD선언이온다.
3. SQL mapper파일은 루트 엘리먼트 &lt;mapper&gt;를 작성하는 것으로 시작한다.  - &lt;mapper&gt;의  namespace속성은 자바의 패키지처럼 여러개의 SQL문을 묶는 용도로, mapper파일에 작성되는 모든 SQL문은 &lt;mapper&gt;의 하위에 있어야한다.
4. SIUD, SQL문이 자리한다. 반드시 닫는 태그로 막아준다.

{% page-ref page="xml.md" %}

### jar

* 클래스들이 들어있는 배포용 파일타입
* jar파일 안에 java파일을 그대로 넣는것은 주석도 그대로 들어감으로 주의해야한다.
* jar를 이용해 클래스를 작성한다면 해당 jar가 서버에 존재하는지 확인해야한다.
* jar추가하기 - Build Path -&gt; Cinfigure Build Path -&gt; Laibraries에서 추가

### mapper

* Mapper 인터페이스
* 매핑파일에 기재된 SQL을 호출하기위한 인터페이스이다.
* 패키지이름.인터페이스이름.ID - ID : 매핑할 메서드 이름

{% page-ref page="sqlmapdeptdao-mybatis.md" %}

{% page-ref page="sqlmapempdao-mybatis.md" %}

## Toad

{% page-ref page="toad-t\_orderbasket-decode-and-sum.md" %}

후기 : 내일은 한글날! 잘 쉬고 오자 운동도하고!

