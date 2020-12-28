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

* 게시글마다 고유의 그룹번호를 가져야 하므로 bm\_group이 0인 게시글 데이터는 삭제한다.

```sql
--양 쪽 테이블 모두에 존재하는 게시글
SELECT * 
  FROM board_master_t bm, board_sub_t bs
 WHERE bm.bm_no = bs.bm_no;
```

* 이 sql문은 양쪽테이블 모두에 존재하는 게시글 데이터만을 조회한다.
* 첨부파일이 없어 board\_sub\_t테이블에 올라가지 않은 게시글 데이터는 조회되지 않는다.

```sql
--첨부파일이 없더라도 게시글을 보여줘야 한다. 아우터조인
SELECT * 
  FROM board_master_t bm, board_sub_t bs
 WHERE bm.bm_no = bs.bm_no(+);
```

* 첨부파일의 유무는 사용자의 선택사항이기 때문에 전체 조회시에는 첨부파일이 없는 게시글 까지도 보여줘야 한다.
* outter join을 사용한다.

```sql
--그룹번호 내림차순, 글차수는 오름차순, 순서에 맞게 뽑는다.
-- bm_no 글번호와 bm_group 글 그룹번호는 일치하지 않을 수 있기때문에 따로 채번해야한다.
SELECT * 
  FROM board_master_t bm, board_sub_t bs
 WHERE bm.bm_no = bs.bm_no(+)
ORDER BY bm.bm_group desc, bm.bm_step asc;
```

* 게시글마다 그룹번호를 갖고, 해당 그룹번호안에 여러 댓글을 위한 글 차수가 존재하기 때문에 위와 같이 ORDER BY를 사용해 정렬한다.

### 화면 호출 타입 3가지

* A타입 - 부모창 자식창과 자바스크립트 변수, 함수를 공유해 호출할 수 있다. - &lt;script&gt; function method\( \){ } &lt;/script&gt;
* B타입 - 팝업창, A타입의 자식창

  - opener.location.href="javascript:method\( \)" --1번방

  - opener.location.href="xxx.jsp \| xxx.sp \| xxx.html" --2번방법, html은 잘 사용하지 않는다.

* C타입 - Modal창 A, B와 달리 소스가 하나다.

### var와 let 예약어

```sql
let url="writeForm.sp";
var a = "";
```

* ECMAScript에서 제안하는 let 예약어
* React.js, Vue.js, TypeScript.js를 활용하기 위해서는 let예약어를 사용하는것이 더 안전하다.

### 새 게시글과 댓글의 차이

* 새 게시글 작성 에는 없지만 댓글 작성 에는 필요한 것이 있다.
* 글 번호 :  새 게시글이나 댓글 모두 새로 채번된 번호를 가져야한다.
* 글 그룹번호 : 댓글에 덧글을 작성 할 수 있어야한다. 이미 작성된 그룹번호를 가져와 구분한다.
* 새 게시글의 경우에는 목록에서 작성을 하는 것이고 댓글은 상세보기 화면에서 작성을 하는 것이므로 댓글은 select된 해당 게시글에 대한 정보를 갖고 시작하게 된다.
* 댓글 작성시 작성되는 댓글 뒤에 댓글이 존재한다면 그 댓글들의 step은 +1되어 끼어들기가 이뤄져야 한다.
* 작성되는 댓글과 그룹번호가 같고, read.jsp에서 이미 결정된 step보다 큰 댓글들의 step은 +1로 set, update되어야 하는 것이다. 

## Android Studio : 로그인

