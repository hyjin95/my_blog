---
description: 2020.11.18 - 66일차
---

# 66 Days -

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
* List는 들어오는 값을 정렬해서 갖지만 Map은 그냥 꽂기때문에 읽고 쓰는 속도가 Map이 빠르다.+++++

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

![](../../.gitbook/assets/1%20%2868%29.png)

## 커넥션 풀

