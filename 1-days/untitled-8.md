---
description: 2020.10.21 - 46일차
---

# 46 Days -

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261 : Oracle.com
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5
* 사용 라이브러리 - JQuery

## 복습

### &lt;table&gt;

* DOM의 구조를 보기 위해 크롬의 개발자도구를 확인한다.
* html &lt;table&gt;, java table, easyui datagrid, ......
* &lt;table&gt;태그의 처리주체는 브라우저로 비동기상태이다. - 이미 읽어와서 완료되었으므로 

### &lt;table class&gt;

* table태그 안에는 클래스 속성이 온다.
* **&lt;table class="제조회사-값"&gt;** - "easyui-datagrid"와 같은 값은 JS-JQuery에서 가져오므로 undefined가 일어날 수 있다. - 해당 속성값을 지원하는 언어가 추가되어있어야 한다.

### &lt;table id&gt;

* JS가 table에 접근하기 위해 id를 사용한다. - name은 중복되지만 id는 유일한 값이므로
* &lt;table id="dg-dept"&gt; - id는 값을 가져올 DB의 table명과 같이 해 구분하기 쉽도록 한다.
* 이렇게 id를 부여하는 것은 JS에서 나머지를 구현하려는 것이다. - JQuery를 이용해 $\("\#id"\)로 호출할 수 있다.

