---
description: 2020.12.28 - 92일차
---

# 92 Days -

## 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org, Toad DBA Suite for Oracle 11.5 , Spring, Android Studio
* 사용 서버 - WAS : Tomcat

## Spring : 댓글 작성하기

### 관계지향형 DB

* Oracle과 같은 관계 지향형 데이터베이스 제품
* 테이블 수가 많다는 성격을 갖는다.
* PK-FK참조 무결성 제약조건을 갖는다. - 스키마에 맞지않는 데이터는 입력되지 않는다.
* 인덱스를 활용한 검색속도 향상에 장점이 있다.

### boardList SQL 수정

```sql
DELETE FROM board_master_t
 WHERE bm_group = 0;
 
commit;
```

* 게시글마다 고유의 그룹번호를 가져야 하므로 bm\_group이 0인 게시글 데이터는 삭제한다.

```sql
--양 쪽 테이블 모두에 존재하는 게시글
SELECT * 
  FROM board_master_t bm, board_sub_t bs
 WHERE bm.bm_no = bs.bm_no;
```

* 이 sql문은 양쪽테이블 모두에 존재하는 게시글 데이터만을 조회한다.
* 첨부파일이 없어 board\_sub\_t테이블에 올라가지 않은 게시글 데이터는 조회되지 않는다.

```sql
--첨부파일이 없더라도 게시글을 보여줘야 한다. 아우터조인
SELECT * 
  FROM board_master_t bm, board_sub_t bs
 WHERE bm.bm_no = bs.bm_no(+);
```

* 첨부파일의 유무는 사용자의 선택사항이기 때문에 전체 조회시에는 첨부파일이 없는 게시글 까지도 보여줘야 한다.
* outter join을 사용한다.

```sql
--그룹번호 내림차순, 글차수는 오름차순, 순서에 맞게 뽑는다.
-- bm_no 글번호와 bm_group 글 그룹번호는 일치하지 않을 수 있기때문에 따로 채번해야한다.
SELECT * 
  FROM board_master_t bm, board_sub_t bs
 WHERE bm.bm_no = bs.bm_no(+)
ORDER BY bm.bm_group desc, bm.bm_step asc;
```

* 게시글마다 그룹번호를 갖고, 해당 그룹번호안에 여러 댓글을 위한 글 차수가 존재하기 때문에 위와 같이 ORDER BY를 사용해 정렬한다.

### 화면 호출 타입 3가지

* A타입 - 부모창 자식창과 자바스크립트 변수, 함수를 공유해 호출할 수 있다. - &lt;script&gt; function method\( \){ } &lt;/script&gt;
* B타입 - 팝업창, A타입의 자식창

  - opener.location.href="javascript:method\( \)" --1번방

  - opener.location.href="xxx.jsp \| xxx.sp \| xxx.html" --2번방법, html은 잘 사용하지 않는다.

* C타입 - Modal창 A, B와 달리 소스가 하나다.

### var와 let 예약어

```sql
let url="writeForm.sp";
var a = "";
```

* ECMAScript에서 제안하는 let 예약어
* React.js, Vue.js, TypeScript.js를 활용하기 위해서는 let예약어를 사용하는것이 더 안전하다.

### 새 게시글과 댓글의 차이

* 새 게시글 작성 에는 없지만 댓글 작성 에는 필요한 것이 있다.
* 글 번호 :  새 게시글이나 댓글 모두 새로 채번된 번호를 가져야한다.
* 글 그룹번호 : 댓글에 덧글을 작성 할 수 있어야한다. 이미 작성된 그룹번호를 가져와 구분한다.
* 새 게시글의 경우에는 목록에서 작성을 하는 것이고 댓글은 상세보기 화면에서 작성을 하는 것이므로 댓글은 select된 해당 게시글에 대한 정보를 갖고 시작하게 된다.
* 댓글 작성시 작성되는 댓글 뒤에 댓글이 존재한다면 그 댓글들의 step은 +1되어 끼어들기가 이뤄져야 한다.
* 작성되는 댓글과 그룹번호가 같고, read.jsp에서 이미 결정된 step보다 큰 댓글들의 step은 +1로 set, update되어야 하는 것이다. 

## Android Studio : 로그인

### Activity와 Fragment, Intent

![](../../../.gitbook/assets/1%20%28103%29.png)

* Activity간의 이동에서 값을 유지시키는 방법은 putExtra로 인텐트에 담고 이동한 페이지 Activity에서 getIntent를 통해 꺼낼 수 있다.
* Fragment는 두 종류로 분류할 수 있다. - xml을 갖고 일반 xml처럼 layout을 제공하는 Fragment - Data만을 가지고 DataSet의 역할을 제공하는 List Fragment

### Android : Thread

![](../../../.gitbook/assets/2%20%2878%29.png)

* Android APK를 구동하는데에는 두 가지의 스레드가 사용된다. - 작업 스레드와 UI스레드

### AsyncTask 의의

* 스레드를 활용한 작업을 편하게 만들어주는 클래스 AsyncTask클래스를 상속받은 클래스는 스레드를 위한 동작코드와 UI접근 코드를 한번에 가질 수 있게되어 사용자 인터페이스에 대한 비동기 처리를 허용해준다.
* 작업 스레드에서 작업을 실행해 결과를 UI스레드에게 전달하는 역할을 수행한다.
* 어떤 작업을 수행하기 위해 새로 만들어진 스레드는 UI객체에 직접 접근할 수 없어 핸들러를 필요로 하는데 그 과정을 줄여준다.
* 작업마다 스레드로 처리했던 코드들을 AsyncTask를 상속받는 클래스를 정의할 수 있게 된다.
* AsyncTask&lt;Params, Progress, Result&gt; - Params : 실행시 작업에 전달되는 값의 타입 - Progress : 작업의 진행정도를 나타내는 값의 타입 - Result : 작업의 결과값의 타입 - 이 중 필요하지 않은 타입이 있다면 Void로 표시하면된다.

### AsyncTask 메서드

* excute\( \) 생성한 AsyncTask를 상속받는 클래스를 실행시키는 메서드 개발자는 UI스레드에서 필요시마다 이 메서드를 호출해 백그라운드 작업 수행을 시작한다. 작업스레드가 해당 작업을 수행하므로 UI객체에 접근 할 수도 있다.
* cancle\( \) 작업을 취소하는 메서드
* doInBackground\( \) 이 메서드 안에는 작업 스레드가 수행한 작업을 작성하고  메서드는 스레드 풀의 작업 스레드를 가져다 이용한다.  파라미터로 배열을 받아올 수 있다. 작업 중간에 UI스레드에 접근하려면 publishProgress\( \)를 호출하고 그러면 UI스레드 내부에서 onProgressUpdate\( \)메서드가 호출된다. UI스레드를 불러와 작업스레드에서 UI작업을 할 수 있다.
* onPreExcute\( \), onProgressUpdate\( \), onPostExcute\( \) 작업 스레드에서 실행되는 메서드들로 UI객체에 자유롭게 접근이 가능하다. - onPreExcute\( \) : 백그라운드 작업 전에 호출되어 작업 스레드에서 초기화 시에 사용된다. - onProgressUpdate\( \) :  백그라운드 작업 진행중에 UI객체에 접근할 때 사용한다.                                            doInBackground\( \)의 진행중에 - onPostExcute\( \) : 백그라운드 작업이 끝나면 호출되고 메모리 리소스를 해체하는 작업을 수행한다.                                  백그라운드 작업의 결과를 매개변수로 전달 받을 수 있다.
* onCancelled\( \) AsyncTask객체를 cancel\( \)로 종료시키면 호출되는 메서드
* doInBackground\( \)에서 백그라운드 작업을 수행하는데 이 메서드를 제외하고서는 다른 메서드는 모두 작업 스레드에서 작업을 수행하면서 UI에도 접근이 가능하다.

### 로그인과 Thread

* 안드로이드에서 제공하는 화면\(XML\)에서 사용자가  id와 pw를 입력하면 Http프로토콜을 활용해 POST방식\(get방식은 보안상의 문제가 발생 할 수 있다.\)으로 Tomcat이 Oracle을 통해 로그인 결과를 다시 안드로이드에 가져와 화면에 표시해주는 작업이 필요하다.
* 이때 xml 화면에 접근해 동기작업을 하는데에 필요한것이 UI스레드이다. 

