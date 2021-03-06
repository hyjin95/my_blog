---
description: 2020.09.14 - 23일차
---

# 23 Days - 서브쿼리, seq, 인스턴스화 위치

## 복습

### 값

* java 변수 = oracle 컬럼
* JTextField에 입력된 값은 숫자를 입력했더라도 String타입이다.
* 값은 UI를 통해 수집한다.

### 값 저장

* 타입을 맞춘다.
* oracle, RAM, 세션&쿠키
* RAM - stack : 일시적 기억으로 값이 사라진다. - heap : Candidate직전까지는 유지된다. - Candidate : A a = null;

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261 : Oracle.com
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5

## 필기

### oracle INSERT

* INSERT INTO 테이블명 \(컬럼, 컬럼, ...\) VALUES \(값, 값, ...\)
* 테이블은 집합으로 SELECT문이 들어올 수 있다.
* 컬럼과 값의 타입이 같아야한다.

### Toad 테이블 복제

* 기존 테이블을 복제해서 새 테이블을 생성하는 경우, 데이터 까지 복제되고 인덱스는 복제되지 않는다.

### PK-FK참조 무결성제약 조건

* 자식 집합에 들어있는 부모 값을 삭제할 수 있다.
* 부모 집합에서 자식 집합에 물려준 값은 삭제할 수 없다. - 부모 값 없이 자식 값은 유지되지 않는다.

### 서브쿼리 종류

* **서브쿼리** : WHERE절에 집합\(SELECT문\)이 들어오는 것
* **인라인뷰** : FROM 절에 집합\(SELECT문\)이 들어오는 것
* 간접조건을 주는 경우에 사용한다.

### Join

* equal 조인 : FK join, AND - SELECT a.회원이름    FROM 회원a, 와인b, 판매c    WHERE a.mem\_id = c.mem\_id    **AND** b.wine\_name = c.wine\_name

### 개발 순서

* 분석\(업무분석\) -&gt; 설계 -&gt; 개발 -&gt; 테스트
* 설계 : 점검이 가능하다. - DB설계 : ER-Win - 클래스 설계 : UML - 화면 설계

### DB설계 & 행위엔티티 & 제로베이스

* DB설계 예시 - P452 - 회원과 와인의 관계는 다대다 이다. - 다대다의 관계에서 조인하면 쓰레기 값이 나올수 있다. - **행위엔티티** '판매'를 넣어 1대다, 다대1 관계로 만들어준다.
* 제로베이스 O - 주문하지 않는 고객이 있을 수 있다. 회원과 판매의 관계에는 제로베이스가 있다. - 선택받지 못하는 와인이 있을 수 있다. 와인과 판매의 관계에는 제로베이스가 있다.

{% page-ref page="toad.md" %}

{% page-ref page="and-and.md" %}

### 인스턴스화 & LifeCycle

* 순제어 : 개발자 \|\| JVM이 LifeCycle을 해준다.
* 역제어 : LifeCycle을 누가 대신 해준다.\(제어역전\)

후기 : 사회적 거리두기 2.5단계가 완화되어 2주만에 대면수업을 시작했다. 영화예매 프로젝트도 거의 끝나간다.

