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
* &lt;table id="dg-dept"&gt; - object이름-id명 - id는 값을 가져올 DB의 table명과 같이 해 구분하기 쉽도록 한다.
* 이렇게 id를 부여하는 것은 JS에서 나머지를 구현하려는 것이다. - 바로 닫힌 태그로 html에서 닫는다. - JQuery를 이용해 $\("\#dg-dept"\)로 호출할 수 있다.
* $\("\#dg-dept"\).datagrid\( \){ } - 위와 같이 접근한 id를 소유주로 object를 선언, 구현할 수 도 있다.

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

## WAS와 웹서버

### WAS

### 웹서버

### 차이점

