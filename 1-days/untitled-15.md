---
description: 2020.10.28 - 51일차
---

# 51 Days -

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5
* 사용 서버 - WAS : Tomcat

## 복습

* **오라클** - Select를 시작으로 컬럼에 접근한다.
* **자바**\(처리주체-서버\) - 객체지향언어, 컴파일한다 - List&lt;Map&gt;에 n개의 값을 담는다. - List : index, Map : key
* **HTML**\(처리주체-클라이언트\) - 절차지향언어, 컴파일\(문법체크\)하지않는다. - 화면, 입력 값을 읽고 응답을 화면에 쓴다. - html : input, easyui : textbox - easyui.js와 같은 API를 사용하면 화면 생성시 별도의 태그를 생성해준다. - html **id는 JS**가 사용한다. **name은 서버\(자바-servlet,jsp\)**에서 사용한다. - name으로 JAVA와 소통한다.
* **JS** - 동적처리의 시작, html의 event담당 - &lt;head&gt;: 호출, &lt;body&gt;:선언 후 -&gt; 시점 설계가 중요하다. - &lt;head&gt;는 시점에 호출하는 것이고, &lt;body&gt;는 순서대로 실행된다. - &lt;body&gt;에서 id에 접근시 선언 위에 코드를 작성하려면 onload\(JS\), ready\(Jquery\)사용 - 페이지 이동시 파라미터로 data를 넘기고 응답을 리턴한다.

## deptList

### 작업지시서

* dataset으로 소통, 출력이 이루어져야한다. ui솔루션을 사용해 다양한 표현을 해보자
* emp.json\(xml\)의 dataSet\(xml\)을 사용해본다.
* JS기반의 ui 솔루션 : easyui\(API\), bootstrap\(반응형 웹\)
* empManager.html 로 페이지를 생성한다. - 2단계 : empMagager.jsp
* html에 테이블 생성 - html : table, easyui : datagrid\(Object로 제공\)
* 바인딩 - &lt;input class="easyui-datagrid" \|\| "easyui-textbox"/&gt; - easyui를 사용하면 개발자가 부여한 input태그의 id는 처음 로드되었을 때만 살아 있고, ready나 onload, event 등이 발생하면 easyui가 제공하는 id로 덮여 씌워진다.

### &lt;label&gt;

* html - &lt;label&gt;이름&lt;/label&gt;   &lt;input type="text"&gt;
* easyui - &lt;input label="이름"&gt;

