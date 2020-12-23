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

### Sequence.nextval

```sql
SELECT seq_board_no.nextval bm_no FROM dual;
```

* 시퀀스가 마지막으로 생성한 번호의 다음 번호를 조회한다.

### Index 생성

```sql
--그룹번호 채번하는 쿼리문짜기
CREATE INDEX iboard_group ON board_master_t(bm_group);
```

* 테이블의 아무 컬럼에도 index를 생성할 수 있지만, PK가 아니라면 유니크한 값이 아닐 것이다.

### index 검색

```sql
SELECT empno FROM emp; 
```

* emp테이블의 empno는 PK이고, PK는 index를 갖고 있다.
* index를 부여해 index를 활용한 검색을 하는 것은 검색시 색인을 이용해 빠른 검색이 가능하게 하는 것과 같다.

### index+일반컬럼 검색

```sql
SELECT empno, ename FROM emp; 
```

* empno는 PK이지만 ename은 일반 컬럼으로 index를 갖고 있지 않다.
* 이렇게 index를 갖지 않는 컬럼을 조회할때에는 옵티마이저가 테이블을 전체 스캔하는 과정을 거치기 때문에 데이터량이 많을 수록 속도가 느려질 수 밖에 없다.
* 그러므로 nosql은 index를 제공하지 않아 대용량 데이터 처리에는 적합하지 않은 것이다. local data를 처리, 기록하는데 적합하다.

### Where 색인 조건

```sql
SELECT empno, ename FROM emp
 WHERE empno>10;
```

* index를 조건으로 하는 쿼리문은 index를 활용한 검색을 한다.

### index와 힌트문

```sql
SELECT /*+index_desc(board_master_t iboard_group)*/ bm_group
  FROM board_master_t
 WHERE bm_group > 0;
```

* 위에서 생성한 index인 iboard\_group를 힌트문에 넣어 색인 검색을 할 수 도 있다.
* 하지만 이경우에는 만드시 where문에 항상 일치하는 조건이라도 index를 갖는 컬럼에 부여해야한다.

### null과 NVL

```sql
-- data가 한건도 없다면 위 쿼리문은 null이 출력된다. 그래서 사용하는 것이 NVL
SELECT NVL(comm,0) FROM emp;
```

* 조회 결과가 null이라면 해당 결과를 활용하는 부분에서 에러가 발생할 수 있다.
* 이럴때 사용하는 것이 NVL\( \)중복검사 구문이다.
* comm

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

### appBar -&gt; toolBar

```java
ActionBar toolbar = findViewById(R.id.아이디값);
setSupportActionBar(toolbar);
```

* toolbar\_main.xml과 menu\_main.xml을 활용해 앱바를 꾸며본다.

### parent 구조

```java
< activity android:name=".OrderActivity
  android:parentActivityName=".MainActivity">
```

* Main과 Order페이지를 부모와 자식 페이지로 구성해본다.
* AndroidManifest.xml
* 장점 여러개의 복잡한 액티비티 사이에서  UPButton을 사용해 한번에 부모페이지로 이동 할 수 있다.

### UpButton추가

```java
 ActionBar actionBar = getSuppoertActionBar( );
 actionBar.setDisplayShowHomeEnabled(true);
```

* Maven레파지토리는 xml에 선언해두면 의존성주입에 따라 jar파일이 자동으로 다운로드되는 Build방법
* gradle은 url-pattern처럼 관리하는것

## Android Studio : PizzaApp Ver1

## Android Studio : PizzaApp Ver2

