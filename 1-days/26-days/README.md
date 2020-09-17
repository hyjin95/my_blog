---
description: 2020.09.17 - 26일차
---

# 26 Days - DISTINCT, DUAL, NULL, ALIAS, NVL, TO\_CHAR, ORDER BY, LOOP

## 복습

### JVM과 Oracle의 값 유지

* 화면에서의 사용자 요청 값을 JVM이 인식하여 DB에 요청하면 Oracle이 처리, 반환한다. - 요청 값이 Oracle Table로 넘어간다.
* 값.getText\( \); - 값을 파라미터로 넘길때, String 타입이다.
* 값.setText\( \); - 처리결과, 리턴값을 보여줄때, void타입이여도 된다.
* 사용자가 요청하는 값은 사용자마다 변화하는 변수로, VO를 사용하는것이 일반적이다. - 변수 = table 컬럼 - ex\) 로그인 : 아이디, 패스워드 

### 전달자

* DML\(SQL\) : PreparedStatement
* PROCEDURE : CallableStatement

### 업무처리

* 함수\(메서드\)의 로직을 이용해 업무를 처리한다. - 리턴과 파라미터를 통해 요청 값이 이동된다. - Oracle함수는 반드시 리턴값과 파라미터를 갖는다. - 함수는 파라미터 값으로 판단, 처리한다. -ex\) Decode

### IF & Decode

* IF 1=1 THEN 실행문 END; - PL/SQL
* Decode\(1,1실행문\) - SQL, DML

### SELECT 실행문

* 위치 :SELECT 실행문 FROM 테이블명
* 실행문 : 산술연산자 + 컬럼 + 함수

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261 : Oracle.com
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5

## Toad

### DISTINCT

### Dual 테이블

![](../../.gitbook/assets/dual-sysdate.png)

### NULL

![](../../.gitbook/assets/1%20%289%29.png)

### ALIAS

![](../../.gitbook/assets/2%20%288%29.png)

### NVL

![](../../.gitbook/assets/3%20%289%29.png)

### TO\_CHAR

![](../../.gitbook/assets/4%20%289%29.png)

### ORDER BY

![](../../.gitbook/assets/5%20%288%29.png)

### LOOP

![](../../.gitbook/assets/6%20%284%29.png)

![](../../.gitbook/assets/6-2.png)

![](../../.gitbook/assets/6-3.png)

{% page-ref page="toad.md" %}

## JAVA

