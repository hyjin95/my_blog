---
description: 2020.12.18 - 87일차
---

# 87 Days - Toad:게시판, 첨부파일테이블, SQL, AndroidStudio:Fragment

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org, Toad DBA Suite for Oracle 11.5 , Spring, Android Studio
* 사용 서버 - WAS : Tomcat

## Toad : 게시판 + 첨부파일 테이블

### SELECT : Equal Join

![](../../../.gitbook/assets/eaual.png)

```sql
SELECT *
  FROM board_master_t bm, board_sub_t bs
 WHERE bm.bm_no = bs.bm_no;
```

* master에는 존재하지만 sub테이블에는 첨부파일이 없을때 이퀄조인을 걸면 양쪽테이블에 모두 존재하는 것만 보여준다.

### SELECT : Outter Join

![](../../../.gitbook/assets/outter.png)

```sql
SELECT *
  FROM board_master_t bm, board_sub_t bs
 WHERE bm.bm_no = bs.bm_no(+);
```

* 하지만 실제로 게시판에서 update된 것은 두 건이므로 sub테이블의 첨부파일이 존재하지 않더라도 master테이블에는 게시글이 있기때문에 이를 보여주기 위해서는 null이 있는 테이블에 \(+\)를 추가한다.

### WHERE : LIKE, AND, OR, NULL

![](../../../.gitbook/assets/1%20%2891%29.png)

```sql
SELECT *
  FROM board_master_t bm right outer join board_sub_t bs
    ON bm.bm_no = bs.bm_no
 WHERE bm_title LIKE '%'||'목'||'%'
    OR bm.bm_content IS NULL;
```

* OR이나 IN의 경우에는 \(+\)를 허용하지 않아 위의 문법을 사용해야 한다.
* AND를 사용할 수록 조건이 많아져 SELECT결과물은 줄어들고, OR는 사용할 수록 조건이 광범위해져 SELECT결과물이 늘어난다.

### UPDATE : transaction, 변수

```sql
UPDATE board_master_t -- 글 삽입에 대한 없데이트, 트랜잭션처리가 필요
   SET bm_step = bm_step + 1
 WHERE bm_group = 35
   AND bm_step > 1;
   
UPDATE board_master_t -- 목록화면인지 상세보기 화면인지, 목록 아닐까
   SET bm_step = bm_step + 1
 WHERE bm_group =:x
   AND bm_step >:Y
```

* 게시글 삽입에 대한 Update 트랜잭션 처리가 필요하다.
* 게시글이 추가되고, 기존 게시글의 순서가 내려가고, 첨부파일이있으면 올려야 한다.

### 전체 조회 SQL

```sql
<select id="boardList" parameterType="map" resultType="map"><!-- bs테이블은 null일 수 있으므로 nullpointer방지로 NVL구문 사용 -->		   
		SELECT bm.bm_no, bm.bm_title, bm.bm_writer, bm.bm_content, bm.bm_email,bm.bm_pw, bm.bm_group, bm.bm_pos, bm.bm_step, bm.bm_hit, bm.bm_date
			   ,NVL(bs.bs_file,'') bs_file, NVL(bs.bs_size,0) bs_size 
		 FROM board_master_t bm left outer join board_sub_t bs
		   ON bm.bm_no = bs.bm_no
		 <where>
		 	<if test="bm_no > 0">
		 		AND bm.bm_no=#{bm_no}
		 	</if>
		 </where>
		 ORDER BY bm_no desc
	</select>
```

## Spring : 계층형 게시판 설계

### 계층형 게시판

![](../../../.gitbook/assets/1%20%2895%29.png)

* 계층형 게시판은 중간에 글 삽입이 일어나면 이전 글은 뒤로 밀려나야하고, 글 번호도 변경되어야 한다.
* 트랜잭션 처리가 필요하다.

### 화면 정의서

![](../../../.gitbook/assets/2%20%2872%29.png)

* 조건검색을 위한 콤보박스와 입력창, 조회 버튼 등이 필요할 것이다.
* 게시판에 사용자에게 보여줄 컬럼과 페이지 네비게이션 바가 필요할 것이다.

### 업무 처리 개발 패턴

![](../../../.gitbook/assets/3%20%2855%29.png)

## Android Studio : fragment

### fragment 파일 생성

![](../../../.gitbook/assets/fragment1.png)

* Activity-java 패키지 우클릭 &gt; New &gt; Fragment &gt; Fragment\(Blank\) : 기본

![](../../../.gitbook/assets/fragment2.png)

* 이름 설정 &gt; Finish

### Fragment로 화면 재사용

{% page-ref page="android-studio-fragment.md" %}

후기 : 파이널 프로젝트의 ERD를 컨펌받는 날이였다. 게시판이 많으니 솔루션으로 구현해보고, 채팅은 웹 소켓과 파이어베이스를 활용해 보라고 하셨다.

