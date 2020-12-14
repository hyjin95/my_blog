---
description: 2020.12.14 - 83일차
---

# 83 Days -



## 필기

### JSP와 인스턴스화

* &lt;jsp:useBean id="tv" class="com.TV scope=" "/&gt; 스코프가 없어 클래스를 유지할 수 없다.
* JSP에서는 인스턴스화하지 않는다.

## Spring

### RequestMapping

* 3.0 이전 Spring - 모두 xml을 이용해 처리했다. - SimpleUrlHandlerMapping, PropertiesMethodNameResolver 이 두 클래스가 있어야 url패턴에 대응하는 메서드 이름을 찾을 수 있었다.
* 5.0 이전 Spring - 주로 자바를 이용해 처리하는 방식으로 변화했다.\(초보자들을 위한 배려\) - 메서드 선언 앞에 어노테이션 @RequestMapping을 사용할 수 있게 되었다. - @GetMapping, @PostMapping

