---
description: 2020.12.23- 90일차
---

# 90 Days -

## 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org, Toad DBA Suite for Oracle 11.5 , Spring, Android Studio
* 사용 서버 - WAS : Tomcat

## Spring : 게시판

### return type String

1. "redirect : xxx.jsp" - 배포위치 webapp에 접근한다.
2. "forward : xxx.jsp" - 배포위치 webapp에 접근한다.
3. "xxx" - ViewResolver를 사용해 WEB-INF에 접근한다.

### @RequestParam

* @RequestParam 어노테이션을 활용하면 get, post방식 전송시 파라미터에 값이 자동으로 담겨 이전에 어노테이션을 사용하지 않고 request.getParameter\( \);로 받아와 pMap.put\(" ",값\)으로 일일히 담았던 반복되는 구문이 감소한다.

## Spring : 새글-댓글 작성 

### 쿼리문

```sql
SELECT seq_board_no.nextval bm_no FROM dual;
```

* 
### 쿼리문

```sql
--그룹번호 채번하는 쿼리문짜기
--아무 것이나 index를 생성할 수 있다. 하지만 유니크하지는 않다.
CREATE INDEX iboard_group ON board_master_t(bm_group);
```

* 
### 쿼리문

```sql
--부분처리(index=색인)하는 힌트문을 부여해 검색 속도의 향상을 기대한다.
SELECT empno FROM emp; --empno는 pk이므로 index를 읽어 처리한다.
```

* 
### 쿼리문

```sql
SELECT empno, ename FROM emp; --ename은 index가 없어 index를 읽어 실행 할 수 없다. table을 전체 스캔한다. 데이터가 많을수록 오래걸린다.

--그래서 nosql같은 경우는 대용량 데이터 처리에는 적합하지 않다. index를 제공하지 않기 때문에. local data를 기록하는데 적합하다.
```

* 
### 쿼리문

```sql
SELECT empno, ename FROM emp
 WHERE empno>10; --조건이 있으면 또 다르다. 색인 조건
```

* 
### 쿼리문

```sql
SELECT /*+index_desc(board_master_t iboard_group)*/ bm_group
  FROM board_master_t
 WHERE bm_group > 0; --멍청한 조건이 있어야만 index를 활용할 수 있다.
```

* 
### 쿼리문

```sql
-- data가 한건도 없다면 위 쿼리문은 null이 출력된다. 그래서 사용하는 것이 NVL
SELECT NVL(comm,0) FROM emp;
```

* 
### 쿼리문

```sql
SELECT comm FROM emp
 WHERE comm >0; -- 이건되지만 comm=null은 찾을 수 없다. 모르는 값이므로 -> int result = 0; 결과가 없어 1이 될 수 없다.
```

* 
### 쿼리문

```sql
SELECT
    NVL(1,0) +1 --null이라면 0으로 치환해달라,여기에 +1한것이 그룹번호이다. 1는 null이 아니니 2가 나온다.
  FROM dual;
```

* 
### 쿼리문

```sql
SELECT --NVL안의 셀렉트 문은 1,2,3,4 모두를 가져온다 우리가 필요한 것은 가장 마지막 번호이다.
    NVL((SELECT /*+index_desc(board_master_t iboard_group)*/ bm_group
          FROM board_master_t
         WHERE bm_group > 0),0) +1
  FROM dual;
```

* 
### 쿼리문

```sql
SELECT
    NVL((SELECT /*+index_desc(board_master_t iboard_group)*/ bm_group
          FROM board_master_t
         WHERE bm_group > 0
           AND rownum = 1),0) +1 --가장 마지막 번호를 가져온다.
  FROM dual;
```

* 
###  게시판 : 새글-댓글 작성 구현

{% page-ref page="spring.md" %}

## Android Studio : pizza 설계

### MainActivity

* activity\_main.xml

### OrderActivity

* activity\_order.xml
* 구성 - 주문서 작성 - 플로팅 버튼 - UpButton 

### appBar

* toolbar\_main.xml과 menu\_main.xml을 활용해 앱바를 꾸며본다.

### parent 구조

* Main과 Order페이지를 부모와 자식 페이지로 구성해본다.
* AndroidManifest.xml &lt; activity android:name=".OrderActivity                  android:parentActivityName=".MainActivity"&gt;
* 장점 여러개의 복잡한 액티비티 사이에서  UPButton을 사용해 한번에 부모페이지로 이동 할 수 있다.

### UpButton추가

```java
 ActionBar actionBar = getSuppoertActionBar( );
 actionBar.setDisplayShowHomeEnabled(true);
```

## Android Studio : PizzaApp Ver1

## Android Studio : PizzaApp Ver2

