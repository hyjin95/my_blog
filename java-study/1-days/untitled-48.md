---
description: 2020.12.28 - 92일차
---

# 92 Days -

## 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org, Toad DBA Suite for Oracle 11.5 , Spring, Android Studio
* 사용 서버 - WAS : Tomcat

## Spring : 댓글 작성하기

### 관계지향형 DB

* Oracle과 같은 관계 지향형 데이터베이스 제품
* 테이블 수가 많다는 성격을 갖는다.
* PK-FK참조 무결성 제약조건을 갖는다. - 스키마에 맞지않는 데이터는 입력되지 않는다.
* 인덱스를 활용한 검색속도 향상에 장점이 있다.

### boardList SQL 수정

```sql
DELETE FROM board_master_t
 WHERE bm_group = 0;
 
commit;
```

```sql
--양 쪽 테이블 모두에 존재하는 게시글
SELECT * 
  FROM board_master_t bm, board_sub_t bs
 WHERE bm.bm_no = bs.bm_no;
```

```sql
--첨부파일이 없더라도 게시글을 보여줘야 한다. 아우터조인
SELECT * 
  FROM board_master_t bm, board_sub_t bs
 WHERE bm.bm_no = bs.bm_no(+);
```

```sql
--그룹번호 내림차순, 글차수는 오름차순, 순서에 맞게 뽑는다.
-- bm_no 글번호와 bm_group 글 그룹번호는 일치하지 않을 수 있기때문에 따로 채번해야한다.
SELECT * 
  FROM board_master_t bm, board_sub_t bs
 WHERE bm.bm_no = bs.bm_no(+)
ORDER BY bm.bm_group desc, bm.bm_step asc;
```

