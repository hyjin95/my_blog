---
description: 2020.12.29 - 93일차
---

# 93 Days - Spring:첨부파일 업로드, 다운로드, MultipartRequest, Android Studio:로그인처리, DB연동하기

## 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org, Toad DBA Suite for Oracle 11.5 , Spring, Android Studio
* 사용 서버 - WAS : Tomcat

## Spring : 첨부파일

### 파일 업로드 준비사항

* spring : maven repository - cos.jar

### 첨부파일 전송 주의사항

```markup
<form id="f_write" action="/board/boardInsert.sp" method="post" enctype="multipart/form-data">
```

* 반드시 'Form'전송으로 처리해야한다. - &lt;form&gt;태그로 내용을 감싸 enctype을 정의해준다.
* 일반 request를 사용해서는 입력 값을 받아올 수 없다. - cos.jar에서 제공하는 MultipartRequest객체를 사용해야한다.
* 게시글이 insert될때 같이 트랜잭션처리로 묶여서 처리되어야 한다.

### 첨부파일 업로드 구현

{% page-ref page="spring.md" %}

### 첨부파일 다운로드 구현

{% page-ref page="spring-1.md" %}

## Android Studio : 로그인처리

### 설계

* LoginLogic클래스에 doInBackground\( \)로 DB담당 jsp를 호출해 로그인처리 값을 받아오는 작업을 정의한다.
* LoginActivity클래스에서 xml의 로그인 버튼이 클릭되면 execute\( \)메서드를 통해 LoginLogic클래스의 doInBackground\( \)메서드를 실행시킨다.
* 사용자의 id, pw입력값은 LoginActivity클래스에서 담아 execute\(id, pw\)로 넘기면 doInBackground\(String.....strings\) 로 파라미터로 받을 수 있다.
* DB연동은 spring에서 mybatis를 사용하지 않고 기존의 방식대로 작성해본다. 

### Manifest

```markup
    <!-- 인터넷 권한 허용 -->
    <uses-permission android:name="android.permission.INTERNET" />
```

* Spring tool에 작성된 DB연결담당 jsp를 불러와 사용할 것이므로 인터넷 권한을 허용한다.

```markup
    <application
        android:usesCleartextTraffic="true">
```

* jsp를 불러오기위한 Http프로토콜 사용을 허용한다.
* Https url로의 접근은 항상 허용하지만 인증되지 않은 주소인 Http url은 위 코드를 작성해야만 접근할 수 있게 된다.

### Android + Spring 로그인처리\(DB\) 구현

{% page-ref page="android-studio-and-spring.md" %}

후기 : 복습에 프로젝트에..정신없다 정신없어!!!!

