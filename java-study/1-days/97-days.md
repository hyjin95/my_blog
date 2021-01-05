---
description: 2021.01.05 - 97일차
---

# 97 Days -

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org, Toad DBA Suite for Oracle 11.5 , Spring, Android Studio
* 사용 서버 - WAS : Tomcat

## Spring-boot  : 조회수

### Model

* Controller에서 메서드를 추가할 필요없이 상세보기 페이지로 넘어갈때 조회수가 올라가기만 하면된다.

### SQL

```markup
	<update id="hitCount" parameterType="int">
		UPDATE board_master_t
		   SET bm_hit = bm_hit +1
		 WHERE bm_no = #{value}
	</update>
```

* board.xml에 쿼리문 작성

## Spring-boot : 삭제

