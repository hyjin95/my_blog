---
description: 2020.12.17 - 86일차
---

# 86 Days -

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org, Toad DBA Suite for Oracle 11.5 , Spring, Android Studio
* 사용 서버 - WAS : Tomcat

## Toad

### 테이블 + pk 생성

```sql
create table board_master_t
(bm_no number(5) constraints bm_no_pk primary key
,bm_writer varchar2(30) not null
,bm_title varchar2(100) not null
,bm_content varchar2(4000)
,bm_email varchar2(50)
,bm_hit number(5) default 0
,bm_date varchar2(30)
,bm_group number(5) default 0
,bm_pos number(5) default 0
,bm_step number(5) default 0
,bm_pw varchar2(10)
);
```

