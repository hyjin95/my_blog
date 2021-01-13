---
description: 2021.01.13 - 102일차
---

# 102 Days -

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org, Toad DBA Suite for Oracle 11.5 , Spring, Android Studio, Visual Studio Code, Nexacro
* 사용 서버 - WAS : Tomcat

## 필기

### 값 유지

* 변수 - 배열\(고정, 상수, 같은타입\) - 객체배열\(유동\[List&lt;VO&gt;\], 다른타입까지도\) - List\(인덱스, 순서가 있어 속도가 느리다\) - Map\(중복가능, 순서가 업어 빠르다.\) - 시간을 관리하기 위해 쿠키와 세션을 사용한다.
* 주의 사항 - 쿠키는 반드시 페이지에 대한 요청이 있어야 반영된다.   값이 있더라도 브라우저가 새로고침\(주소창 변경\)되지 않으면 반영되지 않는다. \(ajax로는 안된다.\)   사용자의 local에 text로 저장된다. - 세션은 서버에 cashe로 관리되어 용량에 제한이 있다.

### Android에서의 쿠키와 세션

* 활용 불가능 : 네이티브앱 - 로컬 디바이스만 사용한다. - 하드웨어를 지원받아 활용할 수 있다.   카메라, 위치기반, 근거리 통신망, 비콘, 블루투스, QR 등.....
* 활용 가능 : 하이브리드 앱

### html의 form전송

* html내에서 form전송이 일어나면 요청의 변화로 기존의 은 리셋되고 새로운 돔구성이 발생한다.

### Bean

* spring-core.jst 스프링 기반의 프레임워크\(전자정부프레임워크, AnyFrameWork 등\)
* 빈 공장 운영 BeanFactory : spring-beans.jar ApplicationContext : spring-context.jar
* @Bean 어노테이션을 사용하거나 xml에 &lt;bean&gt;태그를 사용해 등록한다.

### DB연동, JDBC

* spring-jdbc.jar mybatis.jar mybatis-spring.jar
* mybaits-spring.jar에서 제공해주는 SqlSessionFactoryBean.java로 커넥션을 맺는다. SqlSessionTemplate.java에서는 selectOne, insert, delete, update, selectList 등을 지원한다.

### trigger

* 활성화 시키거나 비활성화 시키는 작업이지 호출해서 실행시키는 것이 아니다.
* rollback의 대상이 될 수 없다.

## Nexacro

