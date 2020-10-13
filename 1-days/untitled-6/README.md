---
description: 2020.10.13 - 40일차
---

# 40 Days - 예외처리, mybatis와 DB연동, 싱글톤, mybatis 동적쿼리

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

## MyBatis와 DB연동

### MyBatis 동적SQL

* MyBaits프레임워크가 제공하는 기능 중 하나
* 동적SQL = Dynamic SQL
* **사용될 SQL문이 runtime시에 결정**되는 SQL문을 말한다.

### API

| JDBC API | MyBatis API |
| :---: | :---: |
| Connection | SqlSessionFactory |
| PreparedStatment | SqlSession |
| executeQuery | selectOne, selectList, slectMap |
| ResultSet | X |
| While | 자동 |

{% page-ref page="../16-days/" %}

## MyBatisZipCodeSearch

{% page-ref page="untitled/" %}

후기 : 

