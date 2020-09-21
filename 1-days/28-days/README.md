---
description: 2020.09.21 - 28일차
---

# 28 Days - equi&outer join, vue, 인라인뷰

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261 : Oracle.com
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5

## Toad

### Join

* 두 개 테이블 사이에 공통되는 컬럼이 있으면 Join할 수 있다.

### Equi Join\(Natural Join\)

* 공통되는 컬럼의 row 중, 같은 값을 가진 row를 기준으로 한다.
* 값이 같은지만 비교할 수 있다.
* **FROM 테이블명 NATURAL JOIN 테이블명**
* **JOIN-ON :** 임의조건을 지정해야 하거나 컬럼을 지정하는 조인 조건은 ON절에, 다른 검색이나 필터조건은 그 다음에 WHERE절을 사용한다.

### OUTER JOIN

* 조인 조건의 컬럼에 테이블 중 어느 테이블이더라도 NULL값이 있지만 결과로 출력해야하는 경우에 사용하는 JOIN이다.
* '\(+\)'기호를 사용해 NULL값이 존재하는 테이블명에 표시한다.

### NON EQUI JOIN

* 크거나 같다. 또는 범위를 정할 때 사용하는 JOIN이다.

### 

{% page-ref page="toad-t\_giftpoint-t\_giftmem.md" %}



## Java

