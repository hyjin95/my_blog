---
description: 2020.12.23- 90일차
---

# 90 Days -

## 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org, Toad DBA Suite for Oracle 11.5 , Spring, Android Studio
* 사용 서버 - WAS : Tomcat

## Spring : 새글-댓글 작성 SQL

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
