---
description: 2020.12.10 - 82일차
---

# 82 Days -

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5 - Spring
* 사용 서버 - WAS : Tomcat

## Final Project

### 서버

* 팀별 오라클 서버 컴퓨터 결정
* 계정 이름과 비밀번호, SID이름 공유

### Tablespace

* 테이블 스페이스 추가 CREAT TABLESPACE 이름결정 detafile '경로' SIZE 100M;
* 테이블 스페이스 수정 ALTER database detafile '경로' RESIZE 150M;
* 테이블 스페이스 확인하기 SELECT tablespace\_name, file\_name, maxbytes FROM dba\_data\_files WHERE tablespace\_name = '테이블스페이스 이름_'_
* 테이블 스페이스 삭제 DROP tablespace 테이블스페이스명

### 계정 생성

