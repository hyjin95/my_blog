---
description: 2020.10.15 - 42일차
---

# 42Days - web, scope, sqlSession선언과 생성, Android Studio, JSP, interpreter, html이론

### 사용 프로그램

* 사용언어 : JAVA(JDK)1.8.0\_261 : Oracle.com
* 사용Tool \
  \- Eclipse : Eclipse.org\
  \- Toad DBA Suite for Oracle 11.5
* 사용 FrameWork\
  \- MyBatis

## 복습

### Tomcat

* 자바 언어 기반의 웹서버
* java가 설치되어있어야 한다.
* Tomcat서버 - MyBatis Layer -  OracleDB
* xml파일 : SqlSessionFactory, SqlSession\
  properties파일 : 프로퍼티 정보

### transaction

![](<../../../.gitbook/assets/2 (17).png>)

* 4개 메서드 모두 같은 method 메서드를 구현하는 것일때,
* Controller클래스와 Logic클래스에서 method는 1 : 1대응이지만 Dao에서는 구현이 n개일 수 있다.\
  \- n개의 대응 메서드를 한 덩어리로 묶어주는 것 = 트랜잭션 처리
* Dao로 넘어온 method는 일단 대기한다.

## WEB

### web server

* Local : 해당 컴퓨터 안에서만 사용가능
* Web : 다른 디바이스, 밖에서도 접속이 가능하다.\
  &#x20;          동시접속자 처리에 유리해 속도가 빠르다.\
  &#x20;           세션과 쿠기를 가져 시간의 흐름에 따라 유지된다.

### 웹 서비스 개요도

![](<../../../.gitbook/assets/2 (16).png>)

### SqlSessionFactory

* 애플리케이션 scpoe
* 한번 생성된 뒤 애플리케이션을 실행하는 동안에는 존재해야한다.\
  \- 삭제하거나 재생성할 필요가 없이 유지된다.\
  \- 애플리케이션이 실행하는 동안 = 로그인해있는 동안\
  \- 자원, 연결통로의 영속성이 이루어진다.
* 계속 build하게되면 반납이 이루어지지 않으면서 계속 쌓이므로 비효율적이다.
* 애플리케이션 스코프로 유지하는 방법\
  \- 1 : static, 싱글턴패턴, 인스턴스화 를 사용한다.\
  \- 2 : 구글 Guice나 Spring같은 의존성 삽입 컨테이너를 사용한다.\
  &#x20;      프레임워크가 SqlSessionFactory의 생명주기를 싱글턴으로 관리하고, 직접 인스턴스화 하지않고    \
  &#x20;      관리해주는 것을 주입받아 사용한다.

### SqlSession

* 지역변수로서 메서드안에서 객체가 생성되어야한다.\
  \- 해당 메서드 밖에서 유지되지 않도록 http요청 마다 생성하고, 응답 리턴마다 sqlSession을 닫는다.\
  \- 응답이 끝날때까지는 유지되어야한다.
* 기능 : commit, rollback, 요청
* 제공메서드 : insert, delete, update, selectOne, selectList, selectMap\
  \- 메서드를 사용하기 위해 인스턴스화 해야한다. = scope필요
* 각각의 thread는 자체적으로 SqlSession 인스턴스를 가져야한다.\
  \- WAS를 사용하면 thread를 구현할 필요가 없다.\
  \- SqlSessionFactory의 db연결이 싱글톤이므로 같은 연결통로를 사용하는 thread들은 각기 다른 session을 가져야한다.\
  \- 하나의 연결통로를 계속 사용하면 session갯수가 초과될 수 있으므로 반드시 finally에서 close한다.\
  \- 응답 반영을 위해 close이전에 commit을 실행한다.

### WAS

* Web Application Server
* Tomcat이 대표적
* 안에 Thread가 내장되어 멀티스레드까지도 관리해준다.
* WAS를 사용하면 thread를 직접 구현할 필요가 없다.

### Web scope

![](<../../../.gitbook/assets/1 (25).png>)

* **Scope** \
  ****- 변수를 어떤 범위내에서 사용할지를 정하는 기\
  \- 사용방법 : 값 저장-setAttribute, 값 읽기-getAttribute
* **Page** 영역\
  \- 해당 페이지 에서만 유지\
  \- JSP페이지에서 pageContext라는 내장 객체로 사용가능하다.
* **Request** 영역\
  \- http 요청을 WAS가 받아서 응답이 끝날때까지 유지
* **Session** 영역\
  \- 웹 브라우저에 접속되어있는 그 시간동안에 유지\
  \- 삭제, 재생성 할 필요가 없다.\
  \- SqlSession이 해당된다.
* **Application** 영역\
  \- 애플리케이션이 실행되는 동안에 계속 유지된다, 전원이 OFF될 때 까지\
  \- SqlSessionFactory가 해당된다.

### JSP

* Java Server Pages
* Java기반의 서버 사이드 스트립트 언어로 웹 서비스를 제공해준다.\
  \- 서버쪽의 스크립트 언어
* HTML코드에 java코드를 넣어 동적 웹페이지 생성을 지원하는 웹 애플리케이션 도구이다.
* 웹 서버(Tomcat 등) WAS가 만들어 놓은 객체들을 사용한다.\
  \- request, session, ....

### JSTL

* JSP Standard Tag Library
* JSP표준 라이브러리
* HTML코드안에 JAVA코드를 사용할 수 있어 코드 작성이 쉽다.
* 수정된 경우, 재배포 할 필요없이 WAS가 처리한다.

### 서버 사이드 스크립트

* Server Side Script
* JSP, ASP, PHP, node.js, Python등..이 해당한다.
* 웹에서 사용되는 스크립트 언어 중, 서버 쪽에서 실행되는 스크립트언어를 말한다.
* 서버와 클라이언트가 통신할때, 서버측에 있는 스크립트 형태의 프로그램 클라이언트가 서버에 하는 요청으로 서버 특정 기능 호출시 서버내에서 동작되는 기능들\
  \- DB접속, 내부 로직 등...\
  \- 서버내에서 동작하지만 클라이언트에게까지 코드가 내려오지는 않는것들\
  \- 반환값이 전송될 수는 있다.&#x20;

## MyBatis delete, multiDelete, Update 구현

### 준비

1. toad에서 sql문 단위 테스트
2. SqlMapDetpDao에서 구현
3. 반복되는 sqlSessionFactory 통로연결은 생성자에서 호출하여 사용한다. = 싱글턴 패턴으로 생성\
   \- 애플리케이션 scope

### 주의사항

1. SQL문이 있는 XML문서의 물리적 위치를 등록(매핑) 했니?\
   \- dept.xml\
   \- 매핑이 이루어지지 않으면 찾을 수 없다.\
   \- 매핑 등록을 해야하는 문서 : MappingConfig.XML, Configuration.xml\
   \- Configuration은 프로퍼티를 프로퍼티 파일로 관리하므로 $기호로 받아온다.\
   \- 변수 : 컬럼(참조형)-Object , 값(원시형)-value
2. namespace와 id는 중복되어서는 안된다.
3. parser...Exception, SAX...Exepction은 XML에러이다.

### Connection.setAutoCommit( ) 메서드

* java.sql.Connection인터페이스의 setAutoCommit 메서드
* 소유주에 대한 트랜잭션이 실행되고 하나를 실행했을떄, 자동으로 commit해주는 기능을 가졌다.
* JDBC의 dafault값 = true(켜짐)&#x20;
* mybatis에서는 openSession메서드안에서 설정할 수 있다.\
  \- openSession(false); : default값으로 commit을 해주어야 commit 된다.\
  \- openSession(true); : 자동 commit

### mybatis.config

* mybatis Layer 설정파일
* 연결에 필요한 프로퍼티를 담는다.
* SqlSessionFactoryBuilder는 XML설정파일에서 SqlSessionFactory의 인스턴스를 빌드한다.

### 구현

{% content-ref url="delete-mutidelete.md" %}
[delete-mutidelete.md](delete-mutidelete.md)
{% endcontent-ref %}

{% content-ref url="update.md" %}
[update.md](update.md)
{% endcontent-ref %}

## HTMl

### HTML

* html파일은 Web Content파일이다.
* 정적 페이지로서 단방향 소통만 지원한다.\
  \- JavaScript를 이용해 양방향 소통을 한다.\
  \- HTML 안에 JavaScript를 사용할 수 있다.
* 화면일 뿐이므로 JSP의 수정사항이 바로 적용되지 못하고 새로고침을 통해야한다.
* client의 요청을 서버에 전송할 수는 있다.\
  \- get방식 : 쿼리스트링, 노출된다.\
  \- post방식
* 전송할 개체를 mark태그로 감싸줘야한다.\
  \- \<form> 개체, 개체, ... \</form>\
  \- action = 목적지, 입력받은 정보를 이동시킬 곳\
  \- submit : 전송요청
* 코드에는 수많은 태그들이 있지만 화면에는 전혀 보이지않는다.\
  \- 인터프리터 방식이기때문
* Hyper : dynamic한, 페이지가 이동하는
* Text : mime타입의 main타입
* html확장자로, 브라우저에 쓰기가 가능해 화면을 구현할 수 있는 파일이다.\
  \- mime타입이 HTML이여야 브라우저가 플러그인(실행) 한다.\
  \- JS만으로도 화면을 구현할 수는 있지만 번거롭다.
* html말고 UI/UX솔루션을 이용해 화면을 구현하는것이 요즘 추세

{% content-ref url="../untitled-5/" %}
[untitled-5](../untitled-5/)
{% endcontent-ref %}

### interpreter

* 코드를 한줄 씩 읽어 내려가며 기계어로 번역해 실행하는 프로그램 방식이다.
* 대표적으로는 Python이 이 방식을 사용하는 언어이다.
* 반대되는 방식인 컴파일러는 소스코드를 번역해 실행파일을 제작하므로 프로그램에 수정사항이 발생한다면 소스코드르 다시 컴파일 해야해서 소스가 커질수록 오래 걸린다.
* 인터프리터는 소스 코드를 수정해서 실행시키면 끝이다.

### status

* **status**\
  \- 200 : 최초 요청시 서버를 경유해 처리를 완료했다.\
  \- 304 : 두번째 같은 요청 시 서버 경유없이 처리를 완료한다.
* 서버가 off상태라면 '사이트에 연결할 수 없음' 메세지가 출력된다.

{% content-ref url="../untitled-5/" %}
[untitled-5](../untitled-5/)
{% endcontent-ref %}

{% content-ref url="html.md" %}
[html.md](html.md)
{% endcontent-ref %}

## Eclipse 웹 프로젝트

### Eclipse 웹 프로젝트 파일 생성하기

* File -> New -> Dynamic Web Project 선택\
  &#x20;  프로젝트 이름 지정\
  &#x20;  프로젝트 위치 지정\
  &#x20;  target runtime, 사용할 웹 서버 선택\
  &#x20;  \-> Next -> Next\
  &#x20;     프로젝트 파일 밑에 파일생성 페이지\
  &#x20;     Generate web.xml deployment descriptor 체크 \
  &#x20;     \-> Finish
* deployment descriptor = 배치서술자, dd 파일
* 프로젝트 파일안에는 jaca코드를 배포한다. = src파일

### HTML파일 생성하기

* 프로젝트 하단의 WebContent파일 우클릭 -> New -> Folder(HTML담을 폴더 생성)
* 생성된 폴더 우클릭 -> New -> HTML File

### UTF-8 설정하기

* Window -> Preperences -> General -> Content Types -> Text\
  &#x20; \- HTML, JavaScript Source File, JSON, JSP항목들을 UTF-8로 encoding -> Update

### 연결 브라우저 선택하기

* Window -> Web Browser -> Chrom

### 서버 xml설정 파일 지정

* 서버 제품의 물리적 위치에 하나, eclipse java안에 생성된 server-server.XML파일이 또 있다.
* 서버 더블클릭 -> overview -> serverOption의 publish module contexts to separate XML files 체크 \
  \-> 저장

### 프로젝트 이름을 간단화하기

* 프로젝트 이름을 매변 변경하기 번거로울때 사용하는 방법
* Server -> modules -> html path 클릭 -> Edit -> path : / 으로 설정 -> OK

## 루트문서 dtd 메서드 사용해보기

### markup languge

* html, xml 파일에서 사용하는 형식이다.
* \<!Element와같이 !태그로 시작하는 곳은 선언문과 같다.\
  \- 이 안에 선언된 것들은 단독으로 사용이 가능하다.
* ATTLIST = AttributeList = 속성\
  \- 이 안에 선언된 것들은 단독으로 사용이 불가능하다.

### dtd 루트파일

```markup
<!ELEMENT parameterMap (parameter+)?>
<!ATTLIST parameterMap
id CDATA #REQUIRED
type CDATA #REQUIRED>

<!ELEMENT resultMap (constructor?,id*,result*,association*,collection*, discriminator?)>
<!ATTLIST resultMap
id CDATA #REQUIRED
type CDATA #REQUIRED
extends CDATA #IMPLIED
autoMapping (true|false) #IMPLIED>
```

### dept.XML

```markup
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="oracle.mybatis.DeptMapper">
 
 <resultMap id="rdvo" type="com.vo.dept"> </resultMap>

 <parameterMap id="pdvo" type="com.vo.dept"></parameterMap>
</mapper>
```

### Configuration.XML

```markup
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration
 PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
 "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
	<properties resource = "com/util/Config.properties"/>		 
   <typeAliases>
 	  <typeAlias alias="dVO" type="com.vo.DeptVO"/>
   </typeAliases>
</configuration>
```

## Android Studio

### AppcompatActivity제공 클래스

* setContentView = initDisplay
* onCreate = main
* LinearLayout = borderLayout, gridLayout같은 것
* xmlns = xmlnamespace\
  \- xmlns:android에서 android는 뒤에오는 구현문들의 소유주를 가리킨다.

### 새 디바이스 만들기

1. 오른쪽 상단의 AVD manager선택
2. 사이즈 설정
3. 버젼 설정

### 애플리케이션으로 내보내기

* build - APK

### activity\_main.XML

```markup
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hello World!"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

</androidx.constraintlayout.widget.ConstraintLayout>
```

* 화면을 구현하는 XML파

### MainActivity.java

```java
package com.example.myapplication

import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle

class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
    }
}
```

* xml을 객체주입받아 실행시키는 주체, java파일

후기 : 이 살인적인 진도...아름다운 나의 노력을 보아라!!! 요즘 진지하게 노트북 들고 지하철에서 정리할까 고민중 입니다 살려주세요. 하핫
