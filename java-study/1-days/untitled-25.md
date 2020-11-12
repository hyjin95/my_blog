---
description: 2020.11.12 - 62일차
---

# 62 Days -

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5
* 사용 서버 - WAS : Tomcat

## 필기

### 개발방법론

* 모델 1 : JSP가 요청처리
* 모델 2 : Servlet이 요청처리
* 모델 2에서 개선된 프레임워크 : Spring, 전자정부F/W, LG의 Daven 등
* UI와 로직-DB는 분리한다.

### @

* 어노테이션

## Servlet, Jsp, 페이지이동

### Servlet 기원

* JFrame이전에 JApplet이 html태그안에서 사용할 수 있었다. 
* Server + Applet -&gt; Serv + let = Servlet  - 서버에서 돌아가는 Applet 
* 생명주기 : init - service - destory
* httpServlet이나 GenericServlet 을 상속받아 사용한다. - GenericServlet은 service메서드만 오버라이드 해서 사용해야하므로 post, get을 구분할 수 없다. - 구분할 수 있는 클래스는 GenericServlet을 상속받는 HttpServlet이다.
* service메서드와 post, get메서드에는 통신을 위한 req, res객체가 있어야한다.
* 들어주는 역할 + 응답을 송출해주는 역할 = Server
* req객체로는 PrintWrite를, req객체로는 Redirect를 구현해 사용한다.

### Servlet 매핑

* Servlet인 자바 클래스의 이름을 web.xml 배치서술자 문서에 매핑해줌으로서 의존성 주입한다. - 배치서술자에 매핑할 수 있는 클래스는 java클래스 뿐이므로 jsp는 매핑할 수 없다.
* 개발자가 직접 주입하는 것  - main메서드를 정의한다. - main메서드 자체의 라이프사이클을 갖는다. - 순수 JAVA 코드 POJO방식
* 외부에서 주입받는것 - 외부 프레임워크가 라이프사이클을 관리해준다. - Spring, 전자정부 F/W - 오버라이드 해야하는 추상메서드가 있을 수 있다.

## getParameter 공통코드 구현

### 반복되는 getParameter : 공통코드 구현

```java
HashMapBinder class

public void bind(Map< , > target){
    target.clear( );
    Enumeration en = req.getParameterNames( );
    while(en.hashMoreElement()){
        String str = request.getParameter(" ");
    }
}
```

* **Enumeration은 판정기능**을 갖고 있다. - iterator와 같은 역할, rs.next와 같이 해당 위치에 값이 있는지 없는지를 판정해준다. - true, false
* map.put\( \), list.add\( \)를 사용해 n개의 name을 담는다. - **n개의 name : while** - for는 반복 size를 알아야 하므로 while을 사용한다.
* name : &lt;input name=" "&gt;
* 이렇게 담긴 name들을 **파라미터인 target에 담아**보자 - targer.clear\( \); : 초기화
* **request.getParameter\( \)의 리턴타입은 String**이다. - **getAttribute의 리턴타입은 Object**이기에 헷갈릴 수 있다. CastingException주의하기
* 5번에서 사용되는 req는 Servlet에게서 받은 객체이다.  - 해당 Servlet안에 HashMapBinder를 인스턴스화 해서 req의 원본을 사용해야한다. - **생성자를 통해 받아와야한다.**

## Servlet QnA

### Q1. 책의 p500에 작성된 서블릿은 화면 페이지를 구성해주나요?

* 네
* out.print구문에 태그가 작성되어 있고 mime타입으로 html이라 선언되어 있다.

### Q2. main메서드가 없는 서블릿을 어떻게 실행되나요?

* 배치서술자 web.xml에 url-pattern이 매핑되어 있어서 해당 url요청이 브라우저로 들어오면 WAS가 인터셉트해 매칭되어있는 name의 java클래스 서블릿을 실행해준다.

### Q3 . 서블릿 안에서 페이지 이동을 하는 방법은 무엇이 있나요?

1. response.sendRedirect\( \) - URL이 지정된 문서로 바뀌고, 이후의 자바 코드는 실행된다.
2. RequestDispatcher의 forward\( \) - 화면 URL은 서블릿을 가리키지만 출력되는 페이지 내용은 지정된 jsp에서 출력된다. - req.setAttribute\( \)메서드로 request에 data를 저장할 수 있다.   RequestDispatcher view = request.getReaquestDispatcher\("xxx.jap"\);   view.forward\(request, response\); - jsp에서 request.getAttribute\( \)메서드로 data를 Object타입으로 꺼낼 수 있다.
3. RequestDispatcher의 include\( \) - xxx.jsp의 출력을 포함해서 기존 페이지가 출력한다. URL이 변하지 않고 다음 자바코드도 진행한다.

### Q4. sendRedirect와 forward가 실행되면 servlet의 화면을 볼 수 있나요?

* 아니요
* 지정된 jsp의 화면을 출력한다.

### Q5. req.forward와 res.sendRedirect을 언제 사용하나요?

* select는 forward\( \) - 처리된 결과인 Object\(Map, VO, ...\)를 jsp에 보내서 출력해야 하니까. req에 담아야한다. - 데이터를 화면에 일부 갱신처리하는 경우에 사용한다.
* U \| I \| D 는 sendRedirect\( \) - 데이터를 화면에 갱신처리하면 화면 전체가 갱신된다. - U \| I \| D 의 결과는 0아니면 1이므로 해당 결과값으로 서블릿안에서 1이라면 jsp를 호출하고, 0이면 호출하지 않는 방식으로, 예매완료나 로그인성공과 같은 때에 사용한다. - 정보를 보낼 필요 없을때

