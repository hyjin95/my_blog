---
description: 'Update : 2021.01.09'
---

# 기술소개서

## 정보

* 한영진
* 서울특별시
* 010-8163-7777
* devyoungj@naver.com

### 소개

신입 개발자를 목표로 공부하고 있습니다.  
백엔드에서 시작해 DB, 서버까지도 공부하는것이 목표입니다.

### 활동

* blog : [https://hyjin95.gitbook.io/my-blog/](https://hyjin95.gitbook.io/my-blog/)
* github : [https://github.com/hyjin95](https://github.com/hyjin95)

## 한국 소프트웨어 인재개발원

* 국비 : 한국소프트웨어인재개발원, 김승수 선생님 자바기반 디지털건버전스 응용 SW개발자 양성 6개월과정 수료중 1월 말 수료 예정

### Semi Project

* **Project 1**  
  세미프로젝트로 영화 예매 프로그램 만들기를 진행했습니다.

  학원 수업 18일차에 3주 진행한 영화예매 프로그램 구현입니다.

| 분류 | 사용 |
| :---: | :---: |
| Language | JAVA |
| DB | Oracle |
| Library | ajax, JDBC |

* 기능 로그인 회원가입 현재 상영영화 확인 영화 상세정보 확인 예매 : 날짜, 좌석 선택 예매 확인 예매 취소
* 담당 : DB, server 생성 및 관리, Dao, SQL문 작성, 정의서 작성
* 설명 JAVA로 프론트단 부터 백엔드  부분까지 처리했습니다. HttpServlet을 통한 url처리방식을 활용해 data와 페이지 이동을 처리 했습니다. 비동기 방식 ajax를 활용해 화면에 데이터를 출력했습니다. DBConnection 담당 클래스를 싱글톤으로 관리하며 클래스 내부에서 자원반납까지 처리했습니다. DB는 ER-win과 엑셀로 설계한 ERD를 사용해 Oracle에 테이블을 생성했습니다. SQL문을 직접 작성해 변수를 활용한 CRUD구문을 처리했습니다.

{% page-ref page="java-study/1-days/undefined-3.md" %}

### Final Project

* **Project 2** 파이널 프로젝트로 웹&앱 구현 진행 중입니다.

| 분류 | 사용 |
| :---: | :---: |
| tool | Eclipse, Android Studio, ER-win, Toad |
| Server | Tomcat |
| DB | Oracle, Firebase |
| Language | Java, Html, Css, Javascript, SQL, JSP |
| UI Framwork | bootStrap |
| Framework | Spring, MyBaits |
| Library | ajax, jquery, JDBC |

* 기능\(진행도\) DB 설계 및 test서버 구현\(완료\) POJO : 게시판 솔루션 구현\(90%\) POJO : 사이트 이벤트 및 기능 구현\(70%\) web ui \(90%\) android ui \(10%\) spring-boot이관 \(0%\)
* 담당 : PM, UI-Logic-DB-Server 지원, 발표 및 계획표, 공정표, 화면+기능정의서 작성
* 설명 프론트 단은 html, css, js로 작성하고 백엔드는 JAVA를 활용합니다. 게시판 화면이 많아 게시판 솔루션을 제작합니다. 솔루션에서 결과값을 boolean으로 받아와야하고 변수활용을 위해 부분적으로 JDBC를 활용합니다. DB는 기본적으로는 Oracle을 실시간 채팅 서비스는 Firebase를 활용해 웹&앱 구현 진행중입니다. UI는 bootStrap과 Html, JS, CSS를 활용합니다. POJO로 먼저 진행한뒤 spring-boot로 이관작업을 진행합니다. POJO에서는 xml을 mybatis sql 문서에만 사용합니다. spring-boot에서는 xml에서 설정, DI, Ioc를 관리하고 어노테이션을 활용합니다.

{% page-ref page="java-study/1-days/bobeat.md" %}

