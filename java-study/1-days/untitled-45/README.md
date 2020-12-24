---
description: 2020.12.24
---

# 91 Days -

## 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org, Toad DBA Suite for Oracle 11.5 , Spring, Android Studio
* 사용 서버 - WAS : Tomcat

## Spring : 새글-댓글

### 단계

* 목록보기 -&gt; 글작성 버튼 -&gt; 화면이 열리면 입력받는다. -&gt; 오라클 전송\(insert\) 및 처리 -&gt; 처리결과\(int\) -&gt; 1이면 등록성공 -&gt; 전체조회\(새로고침\) - board/boardList.jsp\(DB경유\)
* 글 번호, 글 그룹번호 채번하기
* 댓글에 또 다른 댓글 처리 기존 글들의 bm\_step 수정\(update\)
* 글등록 처리 -&gt; 전체조회, board/boardList.sp, forward연결 유지,  WEB-INF/views/board/boardList.jsp 글등록 실패 -&gt; 실패 안내 페이지

### 화면처리

1. writeForm.jsp를 만들어 사용한다. -요청처리
2. easyUI - Dialog를 사용한다. - 목록페이지가 열려있는 상태에서 새 글을 작성하는 Dialog를 띄우는 것이기때문에 목록을 새로고침하는데에 어려움이 있다.

### 새 게시글 작성하기 구현 

![](../../../.gitbook/assets/1%20%28100%29.png)

* url : /board/boardList.sp
* 첫 화면, 이미 DB를 경유해 데이터를 갖고 목록을 보여주고있다.

![](../../../.gitbook/assets/2%20%2876%29.png)

* 입력 버튼을 클릭하면 panel창이 열린다.

![](../../../.gitbook/assets/3%20%2857%29.png)

* data를 입력하고 저장버튼을 누르면 하단에 함수가 호출됨을 알 수 있다.

![](../../../.gitbook/assets/4%20%2840%29.png)

* 성공적으로 등록이 처리되면 alert 창이 열리고 확인을 누르면 창은 자동으로 닫힌다.

![](../../../.gitbook/assets/5%20%2829%29.png)

* 다시 보여지는 메인 페이지는 새로고침되어 방금 입력한 게시글이 작성된 것을 볼 수 있다.

### 댓글 작성하기 구현

* read.jsp에서 시작한다. - 오라클을 경유해 기본정보를 갖고있는 화면

