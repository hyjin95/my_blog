---
description: 2020.12.31 - 95일차
---

# 95 Days -

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org, Toad DBA Suite for Oracle 11.5 , Spring, Android Studio
* 사용 서버 - WAS : Tomcat

## 필기

### jar-jar의 의존관계

* spring-context.jar를 프로젝트에 배포해 ApplicationContext.java 객체주입을 받는다.
* ApplicationContext가 동작하려면 스프링 엔진 spring-core.jar가 있어야만 한다. 서로 의존관계에 있는 jar파일이다.

### jar객체 주입

* Maven - dependency - pom.xml 파일로 관리한다.   JVM 자바 가상머신, Maven-version 등을 관리한다. - Maven Repositoty를 사용한다.
* gradle - dependency
* WEB-INF &gt; lib 에 직접 배포, Build Path에서 라이브러리 등록

### POJO 프레임 워크

* POJO프레임 워크는 req, res객체에 의존적이다.
* HttpServlet을 상속받기 때문에 부모 클래스에게 의존적이게 되는 것이다. doGet, doPost 메서드를 반드시 오버라이드 해야한다.
* 의존적이므로 메서드 구현이나 활용등에서 제약이 발생하기 때문에 상속받는 방식은 결합도가 높다.
* 결합도를 낮추기 위해 나온것이 implements를 활용하는 것이다.
* Spring 프레임 워크는은 인터페이스, 추상클래스 중심의 코딩을 지원한다. 독립성, 재사용성을 높이고 결합도를 낮춘다.
* 여기서 더 나아가 Spring-boot에서는 어노테이션을 제공해 더 높은 자유도를 제공한다.

### get, post 방식

* Post방식으로 전송이 이뤄지는 페이지는 url 쿼리스트링을 통한 단위테스트가 불가능하다.
* 그래서 개발의 초기단계에는 get방식으로 두고 테스트를 완료하면 post로 변경하곤 한다.

## Spring  :  context 환경설정

### url 설정

* 기존 방식으로는 web.xml에 \*.do 를 등록해서 인터셉트했었다. - SimpleUrlHandlerMapping - 위 클래스를 사용하지 않으면 doGet, doPost메서드에서 에러가 발생한다.
* 어노테이션을 활용하면 xml이 없어도 된다. - @RequestMapping\(value="xxx.sp", method="GET\|POST"\)   @GetMapping, PostMapping

### 1. xml : context

```markup
	<!-- Resolves views selected for rendering by @Controllers to .jsp resources in the /WEB-INF/views directory -->
	<beans:bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<beans:property name="prefix" value="/WEB-INF/views/" />
		<beans:property name="suffix" value=".jsp" />
	</beans:bean>
```

### 2. Application.properties : context

```sql
spring.mvc.view.prefix=/WEB-INF/views/
spring.mvc.view.suffix=.jsp
```

### 3. JAVA : context

```java
	public void configureViewResolvers(ViewResolverRegistry registry) {//ViewResolverRegistry는 spring-web에서 제공한다. 디펜던시작성하기
		logger.info("configureViewResolvers 메서드 호출 성공");		
		registry.jsp("/WEB-INF/views/", ".jsp");
	}
```

## Android Atudio

### Andoid의 객체주입

```java
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        //부모클래스 메서드 호출, 메서드 오버라이딩, 부모 생성자 호출, 멤버변수 초기화 등을 하기 위함
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        btn_search = findViewById(R.id.btn_search);
    }
```

* 파라미터로 부모클래스의 상태정보를 받아온다.
* 4번에서 부모의 라이프 사이클 메서드를 호출한다. 메서드 오버라이딩을 사용하고, 부모의 생성자를 호출하거나 멤버변수를 초기화할때 반드시 필요하다.

### Activity Life Cycle

![](../../../.gitbook/assets/2%20%2882%29.png)

* onCreate\( \) -&gt; onStart\( \) -&gt; onResume\( \) -&gt; onPause\( \) -&gt; onStop\( \) -&gt; onDestory\( \)
* 안드로이드가 새로운 Activity를 생성했다.

  onCreate\( \) -&gt; onStart\( \) -&gt; onResume\( \)

* 사용자가 뒤로가기를 눌러 해당 Activity를 종료했다. -&gt; onPause\( \) -&gt; onStop\( \) -&gt; onDestory\( \)
* 모든 함수는 콜백 메서드로서 시스템레벨에서 알아서 호출한다.
* 일시 멈춤 상태 - 전화가 왔거나 동영상 재생을 중지하거나 한 상태 - 일시정지 상태에서 실행상태로 돌아오게되면 onResume\( \) 메서드가 호출되고 Resumed 상태가 된다.
* 정지 상태 - 다른 Activity를 실행한 상태. onStop\( \)메서드가 호출된다. - 다시 실행되면 onRestrat\( \)메서드가 호출되고 Create가 아닌 Started상태로 돌아간다.
* 자원\(리소스\) 관리. - onStop상태에서는 사용중인 리소스를 반납하는 것이 일반적이다.   반납이 이루어지지 않은 상태에서 메모리 소모가 계속 발생한다면 예고없이 Activity를 제거한다. - onStop에서 onRestart\( \)메서드가 호출될 때에는 반납했던 리소스들을 다시 생성한다. 

### Activity Life Cycle Method

* onCreate\( \) - Activity의 설정작업이 실행된다. - Activity가 생성되면서 호출되어 이 메서드 안에서 멤버변수의 초기화가 이뤄져야 한다. - 멤버변수에 대한 접근은 setContentView\( \)메서드로 xml지정이 된  이후에 가능하다.
* onStart\( \), onResume\( \) - onCreate\( \)의 실행이 완료되면 바로 같이 호출된다. - onResume\( \)메서드가 호출된 이후 부터 실행상태이며 일시정지, 정지이전까지는 상태를 유지한다.
* onResume\( \) - 이 단계에서 해당 Activity는 화면에 보이면서 동작하고 있다.
* onPause\( \) - 이 함수를 만나 일시정지 상태가 되면 Activity는 화면에 일부가 보이면서 다른 Activity가 Focus된다. - 해당 Activity는 모든 상태를 유지하지만 시스템의 메모리가 낮은 상태가 되면 제거될 수 있다. - 따라서 onPause\( \)메서드에서는 상태를 저장한다.
* onStop\( \) - 이 함수를 만나면 Activity는 정지 상태로 화면에 전혀 보이지 않는다. - 하지만 상태와 정보를 유지하는데 시스템이 메모리가 부족해지만 바로 제거된다.

### onSaveInstanceState

* Activity의 생성시에 파라미터로 상태값을 받는다.
* 화면을 돌리는 등 디바이스의 설정값이 바뀌어 화면을 다시 그려줘야 하는 상황이 오면 onPause\( \) -&gt; onDestroy\( \) -&gt; onCreate\( \) -&gt; onStart\( \) -&gt; onResume\( \) 순서대로 메서드가 호출되어 모든 변수들이 초기화된다.
* 변수를 수정한 상태로 초기화가 되어버리면 작업이 날아가버리게된다. 그래서 존재하는것이 onSavaInstanceState 콜백 메서드이다.
* onDestroy\( \)메서드의 호출 전에 실행되며 함수의 인자에 key-value로 데이터를 담을 수 있다. 이 데이터들이 Activity생성시에 호출되는 onCreate\( \)메서드의 파라미터로 들어가는 것이다.

### onCreate\( \) : super

```java
 @Override
    protected void onCreate(Bundle savedInstanceState) {
        //부모클래스 메서드 호출, 메서드 오버라이딩, 부모 생성자 호출, 멤버변수 초기화 등을 하기 위함
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        //설정 작업 = layout안에 있는 화면 컴포넌트에 대한 리소스 정보를 담는다.
        //해당 액티비티의 설정작업을 하는 영역
        btn_search = findViewById(R.id.btn_search);
        tv_json    = findViewById(R.id.tv_json);
        btn_search.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                sendRequest();
            }
        });
    }//////////////////////////end of oncreate

```

* 2번 : 메서드의 파라미터로 상태값을 받아온다.
* 4번의 super는 상위\(부모\)클래스를 의미한다. 상위클래스의 onCreate\( \)메서드를 오버라이드 한다는 의미의 코드로 onCreate\( \)를 작성하면 빈 Activity를 생성하고, super.onCreate\( \)로 작성하면 액티비티를 만드는 기본적인 코드를 실행한다.
* 그 이후에 5번 setcontentView메서드로 화면으로 그릴 xml을 지정한다.
* xml이 지정된 후에 값들을 초기화한다. layout에 존재하는 컴포넌트들을 리소스로 초기화하고 Activity의 설정작업이 이루어진다.

### Volley

* 앱에서 서버와 Http통신을 할때 HttpURLConnection을 사용하면 요청과 응답이 가능하지만 직접 Thread를 구현해야한다는 단점이 존재한다.
* 이를 보완하기위해 안드로이드에서 제공하는 라이브러리가 Volley이다.
* 이벤트 등의 요청이 발생했을때  Request객체가 RequestQueue에 추가되고 RequestQueue가 알아서 Thread생성 - 서버에 요청 - 응답 까지 전달해준다.
* 받아온 응답은 RequestQueue에서 Request에 등록된 ResponseListener로 응답을 전달해준다. Thread는 물론 Handler로 필요가 없는것이다.

{% page-ref page="android-studio-volley.md" %}

## Android Atudio: JSON

### 학습목표

* Volley API를 활용해 Thread를 사용하지 않고 JSON형식의 데이터를 처리한다.
* 현대에 공공정보를 JSON의 형태로 제공하는 곳이 많으므로 JSON 데이터의 가공, 처리 능력을 키운다.

### JSON

![](../../../.gitbook/assets/2%20%2881%29.png)

* JSON의 형태가 단일 배열의 형태로 있는 경우도 있지만 위와 같이 배열안에 배열이 또 존재하는 경우도 많이 존재한다.
* 위와 같은 List안에 List가 존재하는 JSON을 출력해보자.

### JSON : dependency

![](../../../.gitbook/assets/1%20%28108%29.png)

* Volley API를 라이브러리에 추가해 Thread없이 통신처리를 허용한다.

![](../../../.gitbook/assets/gson%20%282%29.png)

* GSON 라이브러리를 추가해 JSON형태의 데이터를 처리할 수 있도록 한다.

### 



