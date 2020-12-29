---
description: 2020.12.29 - 93일차
---

# 93 Days -

## 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org, Toad DBA Suite for Oracle 11.5 , Spring, Android Studio
* 사용 서버 - WAS : Tomcat

## Spring : 첨부파일

### 추가 jar

* spring-boot : apache.org - commons-fileupload-1.4.jar - commons-io-2.8.0. jar
* spring : maven repository - cos.jar

### 첨부파일 전송 주의사항

```markup
<form id="f_write" action="/board/boardInsert.sp" method="post" enctype="multipart/form-data">
```

* 반드시 'Form'전송으로 처리해야한다. - &lt;form&gt;태그로 내용을 감싸 enctype을 정의해준다.
* 일반 request를 사용해서는 입력 값을 받아올 수 없다. - cos.jar에서 제공하는  

