---
description: 2021.01.06 - 98일차
---

# 98Days -

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org, Toad DBA Suite for Oracle 11.5 , Spring, Android Studio
* 사용 서버 - WAS : Tomcat

## 필기

### UI솔루션

* JS기반 솔루션\(무료\) - easyUI에서는 datagrid에 json형식을 담을 수 있다.
* XML기반 솔루션\(유료\) - jsp와 같은 화면에 섞어 사용할 수 없다. - 해결방법 1 : url로 요청한다. location.href - 해결방법 2 : 탑재된 xml -&gt; js 변환엔진을 활용한다. - 조회결과를 xml포맷으로 출력해 데이터셋이 담아 grid와 매칭해야 한다.
* 솔루션을 활용하는 경우에는 개발자가 부여한 id로 해당 컴포넌트에 접근 할 수 없는 문제가 발생하기도 한다. 솔루션 자체에서 제공하는 UI가 개발자의 컴포넌트를 감싸기 때문으로 개발자도구에서 코드를 확인해 id를 찾아야 한다.

