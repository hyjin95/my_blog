---
description: 2020.11.05 - 57일차
---

# 57 Days - API:textbox-enter+if, &lt;table&gt;과 &lt;div&gt;, AJAX, empManagerHtype:onDblClickRow, dialog

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5
* 사용 서버 - WAS : Tomcat

## 복습

### JS - 절차 지향적

* &lt;body&gt; - $\(document\).ready\(function\( \) {       $\("\#id"\).datagrid\( { - } \); - dom구성이 완료된 후에 바로
* &lt;head&gt; - &lt;script&gt;     function 함수이름\( \) { } - dom구성 완료 이후, 호출시

### 객체 지향 언어

* JAVA, C\#, php, C++등

### 문법에러와 논리에러

* 문법에러 : 컴파일에러
* 논리에러 : Exception\(java\)
* 논리에러가 더 해결하기 어렵다.

### &lt;table&gt;과 &lt;form&gt;

* &lt;from&gt;태그만으로는 정적인 양식이다.
* DB에서 자료를 뽑아 정보를 담아야 동기화된 동적 정보가 되는 것
* DataSet이 form태그와 동기화 되어야 한다.
* form태그로 감싸면 &lt;table&gt;의 정보를 덩어리째 전송할 수 있다.

### DataSet

* Tag과 Java사이에서 data를 담는 구조 - Tag는 DataSet을통해 java언어로 DB에서 data를 가져올 수 있다.
* json, xml, String이 될 수 있다.

### JAVA&JSP,Servlet 통신, 서비스

* JAVA는 Object를 상속받아 로컬통신만을 지원한다.
* JSP와 Servlet은 HttpServlet객체를 받아 웹 통신 서비스를 지원한다.
* JSP와 Servlet은 JAVA언어를 사용하고, JSP와 Servlet이 JAVA파일에 영향을 끼친다.
* JSP, Servlet과 JAVA의 역할을 구분하고 클래스를 분리해 사용해야한다.

### &lt;table&gt; 키워드

* class : 해당 태그의 Object를 표시한다.  - ex\) easyui-datagrid
* data-options : easyui가 제공하는 Object - JS가 class를 가진 태그에 접근할 때에는 Object를 명시해준다. - $\("\#태그id"\).태그의Object\( { 행위 } \);   $\("\#id"\).datagird\(function\( \){ 행위 } \); - 행위 : 함수\( \){ 속성들을 가질 수 있다. 속성: '값', 속성:'값' }

### &lt;table&gt;과 &lt;div&gt;

* &lt;table&gt;을 &lt;div&gt;로 감싸 div의 id로 &lt;table&gt;에 더 쉽게 접근할 수 도있고, Event를 한번에 적용할 수도 있다.
* &lt;div&gt;에도 width설정이 있고, &lt;table&gt;에도 width설정이 있다면, &lt;table&gt;은 &lt;table&gt;의 width 설정을 따른다.

## 필기

### JS 변수선언 위치

```javascript
<script type="text/javascript">

      var g_cnt=0;//멤버변수
      
      function method( ){
            var cnt=0;//지역변수
            i=10;//지역변수
        }
 </script>
```

* 변수에 담긴 정보가 공유해야하는 정보라면 멤버변수로 선언해야한다.

## API활용 - easyui-textbox에 Enter구현

### API

```javascript
var t = $('#tt');
t.textbox('textbox').bind('keydown', function(e){
	if (e.keyCode == 13){	// when press ENTER key, accept the inputed value.
		t.textbox('setValue', $(this).val());
	}
});
```

* 아스키 코드에서 13은 CR : Enter를 가르킨다.

### searchbox와  textbox

* searchbox는 Enter Event를 기본적으로 지니고 있지만 textbox는 Enter Event를 포함하고 있지 않다. API를 활용해 구현해보자
* searchbox는 event를 갖고있으므로 data-options에서 이벤트를 속성으로 지정해줄 수 있다. - data-options="searcher:empSearch3" - 함수이름만으로도 매칭이 가능

### 과정

* 1단계 : 기존의 searchbox에 있는 함수를 복사해 연결한다.
* 2단계 : if문을 사용해 searchbox와 같은 함수에서 사용한다. - 변수를 사용하고, if문을 사용해 합쳐야한다.

{% page-ref page="api-easyui-textbox-enter-event.md" %}

## AJAX\( \)

### Ajax

* Axynchronous JavaScript And Xml 비동기 구현 방식
* 웹 화면 갱신없이 필요한 데이터만 DB와 동기화하는 방식, 부분처리 - ex\) 네이버의 실시간 검색어 순위
* 너무 남발하면 산만해질 수 있다.
* 데이터 형식 : JSON, XML, CSV
* 참고 : [http://www.nextree.co.kr/p9521/](http://www.nextree.co.kr/p9521/)

### Ajax와 easyui-datagrid

<table>
  <thead>
    <tr>
      <th style="text-align:left">ajax</th>
      <th style="text-align:left">easyui-datagird</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">
        <p>$.ajax( {</p>
        <p>url : xxx.jsp</p>
        <p>,success:function(data){</p>
        <p>JS&#xCF54;&#xB4DC;</p>
        <p>}</p>
        <p>});</p>
      </td>
      <td style="text-align:left">
        <p>$(&quot;#dg&quot;).datagrid( {</p>
        <p>url : xxx.jsp</p>
        <p>,onloadSuccess:function(data){</p>
        <p>JS&#xCF54;&#xB4DC;</p>
        <p>}</p>
        <p>});</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p>&#xACB0;&#xACFC; data&#xAC00; &#xC9C0;&#xC815;&#xD55C; type&#xC73C;&#xB85C;
          &#xCD9C;&#xB825;&#xB41C;&#xB2E4;.</p>
        <p>JSON, XML, CSV</p>
      </td>
      <td style="text-align:left">
        <p>&#xACB0;&#xACFC; data&#xC5D0; easyui&#xAC00; &#xAC1C;&#xC785;&#xD574;
          &#xAC00;&#xACF5;&#xB41C;&#xB2E4;.</p>
        <p>Stringify, split, substring, parse&#xB97C; &#xC774;&#xC6A9;&#xD574;&#xC57C;&#xD55C;&#xB2E4;.</p>
      </td>
    </tr>
  </tbody>
</table>

* \( { 속성 } \) : 속성만 올 수있다. JS를 이용한 함수호출이 불가능하다.
* Success같은 속성안의 함수에서만 JS코드를 사용할 수 있다.

## empManagetH - Ajax를 이용한 수정dialog채우기

### 필요한 API선택 및 준비

* 목록에서 수정할 row를 선택한다. : easyui - onDblClickRow\(index,row\)
* 목록에서 바로 data를 가져와도되지만 ajax를 사용해 DB에서 정보를 끌어와보자 - PK값인 EMPNO를 활용한다.
* Ajax의 API를 살펴본다. - [https://api.jquery.com/jquery.ajax/\#jQuery-ajax-url-settings](https://api.jquery.com/jquery.ajax/#jQuery-ajax-url-settings) - data의 type을 설정할 수 있으니 JSON을 사용해보자 - dialog의 여러 input창을 한번에 DB에 전송하기위해 POST방식을 사용해보자
* JS에서 태그의 id에 접근해 data를 작성할 수 있는 함수를 제공한다. - setValue\( \), val\( \) - $\("\#id\).Object\(json\[index\], 컬럼명\) - $\("\#id\).val\(json\[0\], EMPNO\)
* 공유되어야 하는 정보는 멤버변수로 담는다. - 변수는 JS에서 선언할 수 있다. - 선택된 empno값이 수정버튼 클릭시 유지-&gt;넘어가야 한다.

### onDblClickRow\(index,row\)

![](../../../.gitbook/assets/1%20%2859%29.png)

* onDblClickCell\(index,field,value\) - 하나의 cell만 클릭된다. - index : 몇번째row, field : 어느컬럼, value : cell 값
* onDblClickRow\(index,row\) - 하나의 row가 클릭된다.\(해당 row의 n개 cell모두 클릭된다.\) - index : 몇번째row, row : 선택한 row - row.컬럼명 으로 cell값에 접근 할 수 있다.

### dialog와 popup의 차이

* window와 popup은 다른 페이지이다.
* window와 dialog는 같은 페이지이다. - dialog의 내부만 다른 것 - 그래서 hide\( \)해놓고 이벤트 발생시 show,open\( \)해주는 것 - window html에서 JS로 dialog내부 태그의 id에 접근 할 수 있다. 

### Java : 2차배열 - JS \(index,row\)

* JAVA : int is\[0\]\[1\] = 2차배열
* JS : Array, for\( \)\(is\[0\].EMPNO\)

후기 : 이번주는 평일내내 남아서 공부도 하고, 운동도 열심히 했다. 다음주도 이렇게만!

