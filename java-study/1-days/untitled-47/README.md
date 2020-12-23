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

### XML설정 : annotation X

* 장점 - xml문서안에서 모든 업무에 대한 내역을 확인할 수 있다. - xml문서만으로도 전체 클래스의 설계와 업무에 대한 클래스 정보를 확인할 수 있는 것이다. - 계층별로 xml문서를 따로 관리하기때문에 유지보수에도 편리하다. - 객체 주입관계가 직관적이다.
* 단점 - 업무에 새로 투입된 개발자들이 바로 파악하기에 어렵다. - xml문서를 파악할 줄 알아야한다. - 자바와 다른 언어이다. - 진입장벽이 높아 코딩의 시작까지 시간이 늦어진다. - 개발자가 직접 구현하지 않은 클래스들이 많다. - 상속을 받아 처리하므로 의존적이다.
*  표준서블릿 HttpServlet - 메서드 오버라이딩을 준수해 두개의 메서드만이 req, res객체를 주입받아 사용할 수 있다. - 요청, 응답객체에 의존적이다.
* 사용자 입력 값 - Map을 선언하고 request.getParameter\("이름"\); 으로 가져와 map에 put해야한다. - 파라미터로 받아올 값의 갯수만큼 코드가 늘어나고 반복된다. - 이를 돕고자 만들어진것이 HashMapBinder 클래스이다.

## Spring : 새글 작성 - 그룹번호 채번

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
* 이럴때 사용하는 것이 NVL\( \) 구문이다.
* comm컬럼에 값이 존재한다면 그 값을, null이라면 0을 출력해준다.

### WHERE comm = null

```sql
SELECT comm FROM emp
 WHERE comm > 0; 
```

* 조건으로 컬럼 = null 은 찾을 수 없다.
* null을 모르므로 null은 모르는 값이기때문에 +1을한다고 해서 1이 되는 것이 아니다.

### NVL을 활용한 +1

```sql
SELECT
    NVL(1,0) +1 --null이라면 0으로 치환해달라,여기에 +1한것이 그룹번호이다. 1는 null이 아니니 2가 나온다.
  FROM dual;
```

* 새로운 글을 작성할 때에는 마지막 그룹번호에 +1된 값을 그룹번호로 가져야 할 것이다.
* NVL구문을 통해 null이라면 그룹이 하나도 없다는 것이므로 새로 작성되는 글은 그룹번호 1을 가져야 한다.

### 모든 그룹번호 채번

```sql
SELECT 
    NVL((SELECT /*+index_desc(board_master_t iboard_group)*/ bm_group
          FROM board_master_t
         WHERE bm_group > 0),0) +1
  FROM dual;
```

* 결과값은 4, 3, 2, 1
* NVL안의 조회문은 모든 그룹번호를 불러온다.

### 새 글의 그룹번호

```sql
SELECT
    NVL((SELECT /*+index_desc(board_master_t iboard_group)*/ bm_group
          FROM board_master_t
         WHERE bm_group > 0
           AND rownum = 1),0) +1 --가장 마지막 번호를 가져온다.
  FROM dual;
```

* 새로 작성되는 글에 필요한 그룹번호는 가장 마지막 그룹번호에 +1이 된 값이므로 NVL의 조회구문에 5번 조건을 추가한다.
* 결과값은 5

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

