---
description: 2020.12.17 - 86일차
---

# 86 Days -

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org, Toad DBA Suite for Oracle 11.5 , Spring, Android Studio
* 사용 서버 - WAS : Tomcat

## Toad

### 테이블 + pk 생성

```sql
create table board_master_t
(bm_no number(5) constraints bm_no_pk primary key);
```

* pk를 설정하면 index는 자동으로 생성된다.

### index 생성

```sql
create unique index scott.bmno_pk on scott.board_sub_t(bm_no, bs_seq);
```

* pk가 없다면 index를 따로 생성한다.

### FK 제약조건 생성

```sql
--제약조건 안전장치 설정, 제약조건 넣기
alter table scott.board_sub_t add(constraint fk_bmno
foreign key(bm_no)
references scott.board_master_t(bm_no)
enable validate);
```

* FK 제약조건 설정하기

{% page-ref page="untitled-45.md" %}

## Spring

### webapp&gt;board, WER-INF&gt;board

* sendRedirect, forward는 webapp, ModelAndView는 후

### web.xml

```markup
	<!-- Processes application requests -->
	<servlet>
		<servlet-name>spring31</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
		<init-param>
			<param-name>contextConfigLocation</param-name>
			<param-value>/WEB-INF/spring-servlet.xml</param-value>
		</init-param>
		<load-on-startup>1</load-on-startup>
	</servlet>
		
	<servlet-mapping>
		<servlet-name>spring31</servlet-name>
		<url-pattern>*.sp</url-pattern>
	</servlet-mapping>
```

* xxx.sp가 아닌 요청들은 표준 서블릿으로 처리되어 스프링을 경우하지 않는다.  DispatcherServlet을 활용하지 않는다.
* xxx.sp로 들어오는 요청들은 스프링이 관여하고 DispatcherServlet이 활용된다.

### ViewResolver의 관여

* ViewResolver가 관여한다는것은 접두어와 접미어를 제공한다는 것이다. 배포위치는 ViewResolver에 지정된 경로가 된다. WEB-INF/views/ + 페이지이름 + .jsp
* ModelAndView를 사용할 때 -&gt; WEB-INF를 바라본다.
* boot에서는 ModelAndView대신 String을 사용한다. - "redirect:xxx.jsp" -&gt; webapp/board를 바라본다. - "foward:xxx.jsp"  -&gt; webapp/board - "board/xxx."         -&gt; ViewResolver가 관여, WEB-INF를 바라본다.

## Spring 실습 : boot이전

### boot 이전

* xml기반 설정
* 단점 : xml에 오류 발생시 서버가 터져 다른 개발자들도 테스트가 불가능해지는 일이 발생한다.
* 장점 : boot보다 유지보수에 유리한 점이 있다.

### spring3 프로젝트

![](../../../.gitbook/assets/spring3.png)

* 스프링 메뉴에서 제공한 프로젝트로 xml기반으로 설정했지만 이클립스가 제공하는 프로젝트 트리구조를 준수하는 프로젝트 실습

### spring3-1 프로젝트

![](../../../.gitbook/assets/spring3-1.png)

* 스프링 메뉴에서 제공한 프로젝트 이지만 이클립스가 제공하는 xml문서가 아닌 개발자가 직접 xml문서를 정의한 프로젝트 실습
* &lt;init-param&gt;spring servlet.xml&lt;init-param/&gt; 서블릿의 요청 발생시 마다 새로 읽어 처리한다.
* &lt;context\_param&gt;spring-service.xml, spring-data,xml&lt;context-param/&gt; 단독 선언되어 서버가 처음 기동될때 읽은 내용을 유지한다. 

## Spring 실습 : boot이후

### boot 이후

* application.properties기반 설정
* 어노테이션 사용
* 장점 : 개발자로서의 편리함을 제공해준다.

### return Type : String

* return "redirect : xxx.jsp"
* return "redirect : "xxx.sp3"
* return "forward : "xxx.jsp"
* return "board/boardList" - ViewResolver를 사용하는 방법   /WEB-INF/views/+여기에페이지이름+.jsp
* return "foward : "xxx.sp3" X - forward로 새 요청을 하는 것은 허용하지 않는다.

