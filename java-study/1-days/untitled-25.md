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

* 
### 반복 getParameter : 공통코드 구현

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
* **request.getParameter\( \)의 리턴타입은 String**이다. - getAttribute의 리턴타입은 Object이기에 헷갈릴 수 있다. CastingException주의하기
* 5번에서 사용되는 req는 Servlet에게서 받은 객체이다.  - 해당 Servlet안에 HashMapBinder를 인스턴스화 해서 req의 원본을 사용해야한다. - **생성자를 통해 받아와야한다.**

## getParameter 공통코드 구현

