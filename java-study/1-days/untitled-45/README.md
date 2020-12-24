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

{% page-ref page="untitled-50.md" %}

### 댓글 작성하기 구현

* read.jsp에서 시작한다. - 오라클을 경유해 기본정보를 갖고있는 화면

## Android Studio : 작업지시서

### APK명

* Pizza201224.apk

### AndroidManifest.xml : 앱 적재소

* 인터넷 사용권한 추가
* 액티비티 등록
* 앱 아이콘 등록
* 앱 테마 결정\(NoActionBar\)
* 액티비티 간의 관계 결정

### JAVA

* com.example.프로젝트 이름.MainActivity.java - 런쳐 클래스\(시작클래스\)
* com.example.프로젝트 이름.OrderActivity.java - 서브 클래스 - 주문서 용도의 클래스

### res

* res &gt; drawable : 이미지, 아이콘 추가
* res &gt; layout : activity\_main.xml, toolbar\_main.xml\(include\)
* res &gt; menu : menu\_main.xml추가
* res &gt; value  - strings.xml, styles.xml\(이전버전, 사용자 정의 스타일\), color.xml\(주요색상정보등록\), theme.xml

### gradle, maven방식

* maven 방식 - spring에서 권장하는 방식 - maven repository빌드로 진행한다. - pom.xml에 repository정보를 xml에 dependency로 등록해서 원격 자동 동기화 처리
* gradle 방식 - 안드로이드에서 권장하는 방식 - java -&gt; class -&gt; xxx.dex - build.gradle\(프로젝트 수준\)   프로젝트 전반에 대한 빌드정보 추가\(구글서비스 이용 유무 등\) - build.gradle\(앱 수준\)   패키지 정보, 가상머신 정보, 의존성 주입\(관계\), jar등록 등 - gradle.properties   메모리 지정, 메모리 설정, apk에 대한 효율성 체크, androidX패키지 사용 유무 등

### MainActivity.java : 모델계층

* activity\_main.xml\(뷰 계층 : Vue.js, React 등\)
* Activity.class에서 activity.xml에 작성된 id에 접근하기 TextView tv\_id = findViewById\(R.id.아이디\); Toolbar toobar = findViewById\(R.id.아이디\);
* 메인액티비티 R.java : 주소번지가 int로 저장된다.

### ActionBar\(AppBar\)

* 공통코드를 만들어 재사용성을 높여보자
* res &gt; layout &gt; toolbar_main.xml -_ res하위의 정보들은 대부분 @으로 접근할 수 있다.
* xml에서 xml에 접근하기  &lt;Button android:text="@strings/이름"/&gt;.

### 액션바에 툴바 장착하기 : Activity

```java
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        Toolbar toolbar = findViewById(R.id.toolbar);
        setSupportActionBar(toolbar);
    }
```

* xml에 생성되어 있으므로 new Toolbar\( \):하지 않는다.
* 6번에서 선언한 toolbar를 actionBar에 장착한다.

### 메뉴에 대한 리소스 파일 항목 앱바에 추가하기 : Activity

```java
    @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        getMenuInflater().inflate(R.menu.menu_main,menu);
        return super.onCreateOptionsMenu(menu);
    }
```

* 메뉴에 대한 리로스 파일 항목 앱바에 추가하기 - @param1 : 메뉴에 대한 리소스파일 - @param2 : 메뉴 리소스 파일을 자바로 표현한 menu객체

### menu\_main.xml에 등록된 id 이벤트 처리 : Activity

```java
    //액션 아이템 클릭 처리
    public boolean onOptionsItemSelected(MenuItem item) {
        switch (item.getItemId()){//선택된 아이템에 대한 정보
            case R.id.action_create_order:
                Intent intent = new Intent(this,OrderActivity.class);
                startActivity(intent);
                return true;
            default:
                return super.onOptionsItemSelected(item);
        }
    }
```

* menu\_main.xml문서에서 등록한 아이디에 접근해 이벤트를 처리한다.

### 부모액티비티 선언하기 : Manifest

```markup
<activity android:name=".OrderActivity"
            android:label="주문서"
            android:parentActivityName=".MainActivity"/>
```

* 부모액티비티에 대한 선언은 적제소에서 parentActivityName으로 설정한다.
* 이렇게 부모액티비티를 선언해 놓으면 수많은 페이지 중에서도 부모페이지로 바로 이동하게 할 수 있다.

### menu\_main.xml 살펴보기

```markup
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto">
    <item
        android:title="@string/order"
        android:id="@+id/action_create_order"
        android:icon="@drawable/iic_add_white_24dp"
        android:orderInCategory="1"
        app:showAsAction="ifRoom"/>
</menu>
```

* 5번 : strings.xml에 등록된 자원
* 6번 : 자바코드에서 사용할 아이디값 선언
* 7번 : 액션 아이콘 이미지 값
* app:showAsAction="ifRoom\|withText\|never\|always" - ifRoom : 공간이 있으면 앱바에 추가하고 없으면 오버 플로우에 추가한다

### PizzaVer1

### PizzaVer2

