---
description: 2020.10.21 - 46일차
---

# 46 Days - &lt;table&gt;분석, WAS와 웹서버,

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261 : Oracle.com
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5
* 사용 라이브러리 - JQuery

## &lt;table&gt;

### &lt;table&gt;

* DOM의 구조를 보기 위해 크롬의 개발자도구를 확인한다.
* html &lt;table&gt;, java table, easyui datagrid, ......
* &lt;table&gt;태그의 처리주체는 브라우저로 비동기상태이다. - 이미 읽어와서 완료되었으므로 

### &lt;table class&gt;

```markup
<table class="easyui-datagrid">
```

* table태그 안에는 클래스 속성이 온다.
* **&lt;table class="제조회사-값"&gt;** - "easyui-datagrid"와 같은 값은 JS-JQuery에서 가져오므로 undefined가 일어날 수 있다. - 해당 속성값을 지원하는 언어가 추가되어있어야 한다.

### &lt;table id&gt;

```markup
<table id="dg"></table>
```

* JS가 table에 접근하기 위해 id를 사용한다. - name은 중복되지만 id는 유일한 값이므로
* &lt;table id="dg\_dept"&gt; - object이름-id명 - id는 값을 가져올 DB의 table명과 같이 해 구분하기 쉽도록 한다.
* 이렇게 id를 부여하는 것은 JS에서 나머지를 구현하려는 것이다. - 바로 닫힌 태그로 html에서 닫는다. - JQuery를 이용해 $\("\#dg\_dept"\)로 호출할 수 있다.
* $\("\#dg\_dept"\).datagrid\( \){ } - 위와 같이 접근한 id를 소유주로 object를 선언, 구현할 수 도 있다.

### &lt;table data-options&gt;

```markup
<table class="easyui-datagrid" style="width:400px;height:250px"
        data-options="url:'datagrid_data.json',fitColumns:true,singleSelect:true">
    <thead>
        <tr>
            <th data-options="field:'code',width:100">Code</th>
            <th data-options="field:'name',width:100">Name</th>
            <th data-options="field:'price',width:100,align:'right'">Price</th>
        </tr>
    </thead>
</table>
```

* data-options="이름:값, 이름:값, 이름:값, ....." - 열거형 연산자를 사용한다.
* datagrid안에 들어가는 값을 구현한다. - dataSet과 같다.
* 웹에서는 JSON을 사용해 구현한다. - WAS에 JSON이 추가 되어 있어야 undefined가 일어나지 않는다.
* JSON을 담기 위해 URL을 사용한다. - data-options="URL:파일명, URL:파일명, ..."

### &lt;script JQuery - easyUI&gt;

```markup
<script type="text/javascript" src="https://www.jeasyui.com/easyui/jquery.min.js"></script>
<script type="text/javascript" src="https://www.jeasyui.com/easyui/jquery.easyui.min.js"></script>
```

* JQuery.js를 먼저 script한 다음에 easyUI.js를 script해야한다. - 자바에서는 객체지향언어이므로 상관없지만 HTML은 절차지향적 언어이므로 - 서로 의존관계에 있다.  - 2번이 1번위에 있다면 undefined가 발생할 것이다.
* min : 쓸모없는 공백을 모두 제거해 좀 더 가볍게 만든 파일, 가독성이 떨어진다.

## WAS와 웹서버

### WAS

### 웹서버

### 차이점

