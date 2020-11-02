---
description: 2020.11.02 - 54일차
---

# 54 Days - DataSet타입, text-valueField, JSON-XMl, JSP-Servlet, JSP공백제거,

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5 - Visual Studio Code
* 사용 서버 - WAS : Tomcat

## 복습

### 복습 목표

1. 무엇을, 어떻게 전술세우기
2. 역할 생각하기
3. 교집합 찾아보기
4. 관계 생각하기
5. 이벤트 중심의 코드분석

### data와 html - 2

* 역할에 대해 생각해보자
* html 역할 : 보이는 화면, 단방향\(정적\)
* JS    역할 : 보이지않는 부분, html보완, 양방향\(동적\) - html과 JS는 상호보완적 관계 - 양방향 서비스를 하려면 제어문이 필수\(if, for\)
* data - 자료 : 정제되지 않은, 방대한 자료 - 정보 : 정제된, 필요한 자료
* 화면에 data를 어떻게 반영할 것인가 - dataSet, Json, xml, Servlet - easyui의 data-options = "url:'여기' " - 여기 : php, json, jsp, xml, ....

### servlet과 jsp - 3,4

* req, res내장객체를 서버로부터 주입받아 내장객체로 갖는다.
* Servlet으로는 화면을 구현하는데 불편해 JSP가 탄생
* 클라이언트 요청 - servlet - 서버 - JSP - 클라이언트에게 응답
* JSP는 html에 자바코드를 작성 - WAS를 상속
* Servelt은 자바에 html코드를 작성 - JAVA에 상속

### dataSet

* \[{"id":1, "text":"test"}\]
* 의미 있는 정보는 text 정보
* id는 유일한 값으로 DB의 PK역할, JAVA의 멤버변수 역할과 같다. 

### session, cookie

* session은 시간, cookie는 보안에 취약하다.
* 다른 방법으로 data를 유지할 수 있을까? - JSP

## easyui - combobox

### A, B, C type

* A : html
* B : html + js
* C : js

### A-B차이

* UI 가독성
* 단방향, 양방향 서비스에 어느 타입이 더 유리한지

### combobox - 기본형태1

```markup
<input id="cc1" class="easyui-combobox" data-options="
        valueField: 'id',
        textField: 'text',
        url: 'get_data1.php',
        onSelect: function(rec){
            var url = 'get_data2.php?id='+rec.id;
            $('#cc2').combobox('reload', url);
        }">
```

* html태그 안에 이벤트, url, 속성값들이 있어 JS코드가 필요하지 않다.
* body코드가 길어져 지저분해진다.
* html, JS의 역할 분리가 없어 업무분담을 할 수 없다.

### combobox - 기본형태2

```markup
<input id="cc2" class="easyui-combobox" data-options="valueField:'id',textField:'text'">
```

* 역할을 분리하여 효율적이 되었다.
* JS코드가 필요하다.

### url 역할, xml,JSON : 어떤 dataSet을 사용할까?

<table>
  <thead>
    <tr>
      <th style="text-align:center">JSON</th>
      <th style="text-align:center">XML</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:center">DataSet &#xACE0;&#xB824; X</td>
      <td style="text-align:center">
        <p>DataSet &#xACE0;&#xB824; O</p>
        <p>(&#xCF54;&#xB4DC;&#xAC00; &#xCD94;&#xAC00; &#xB41C;&#xB2E4;.)</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:center">
        <p>JSON API &#xCD94;&#xAC00; O</p>
        <p>(google.gson)</p>
      </td>
      <td style="text-align:center">&#xBCC4;&#xB3C4; API&#xC81C;&#xACF5;, &#xCD94;&#xAC00;X</td>
    </tr>
    <tr>
      <td style="text-align:center">
        <p>&#xACF5;&#xAC04;&#xD655;&#xBCF4; &#xC5C6;&#xC774;</p>
        <p>Map.put &#xBC14;&#xB85C; &#xB2F4;&#xAE30;</p>
      </td>
      <td style="text-align:center">
        <p>dataSet&#xC774; &#xC874;&#xC7AC;&#xD558;&#xBBC0;&#xB85C;</p>
        <p><b>addRow</b>&#xAC00; &#xBC18;&#xB4DC;&#xC2DC; &#xC6B0;&#xC120;</p>
        <p>(&#xACF5;&#xAC04;&#xD655;&#xBCF4;)</p>
      </td>
    </tr>
  </tbody>
</table>

* php, aspx, jsp, do, ...여러 타입의 페이지들이 올 수 있다. - do : servlet
* 역할 - 화면 - dataSet : JSON, XML, String
* dataSet - String은 비효율적이다. - XML : 열린태그, 닫힌태그를 동반해 무겁다. - **JSON** : xml보다 가벼워 빠르다. - 대용량처리가 가능
* 사용할 테이터셋의 valueField에 따라 대문자, 소문자도 구분해야한다.

### dataSet

* dataSet은 **별도의 타입**이 있으므로 타입을 생각해야한다.
* 자바의 int가 orcale의 number인것과 같이 타입이 달라 반드시 매칭작업이 필요하다. - JSON으로 할 것이다.
* JSON이라면 dataSet을 고려할 필요가 없다.
* XML이라면 dataSet을 고려해야만한다. - ex\) 넥사크로, Flex
* DataSet은 data-options로 채워주기 전에는 비어있는 상태이다. - data-options에서 배정된 field에서 헤더를 사용해 가져오는 것 - &lt;th&gt;

### text-value Field

* valueField - pk, 식별자 이름, 조건으로 사용할 값 - \#{deptno}로 넘어가는 값
* textField - 값으로 꺼내 보여줄 data 식별자 이름
* ex&gt; valueField="DEPTNO", textField="DNAME" - 눈에 보여지는 data는 DNAME값 이다. - SELECT dname FROM dept WHERE deptno = ? -&gt; \#{deptno}

### JSP, Servlet : 어떤 프로그램을 사용할까?

* JSP : 모든 타입을 사용할 수 있다. mimetype을 작성하므로 해석하기 좋다. - &lt;%@ contentType=" / " %&gt;
* Oracle을 경유해야하는 경우에는 무엇을 사용해야 할까? - 서블릿 - Oracle을 경유한다는 것은 JAVA코드를 사용한다는 것 - 서블릿으로 데이터를 담고 -&gt; JSP로 넘겨서 JSP가 화면에 응답을 출력해야한다. - senddirect : get방식, 유지되지 않는다  - forward를 사용하면 JAVA와 JSP가 서로 다른 소스지만 유지할 수 있다.

### combobox에 data가져오기

1. html combobox에 url : xxx.jsp설정
2. xxx.jsp 생성 - mim타입 : application/json - oracle연동에 필요한 소스 import - 데이터를 Map, List에 담아 Json형식으로 변환 - out.print로 단위테스트

### JSP공백제거

```markup
<jsp-config>
		<jsp-property-group>
			<url-pattern>*.jsp</url-pattern>
			<trim-directive-whitespaces>true</trim-directive-whitespaces>
		</jsp-property-group>
	</jsp-config>
```

* JSP는 요구에 따라 JSON의 역할 혹은, XML의 역할도 자주 담당한다. 
* 이 때 맨 위의 white space가 들어가는 문제가 발생한다. 
* web.xml 배치서술자 문서에 처리하면 jsp문서 모두에 대해 제거 일괄적용이 가능하다.

## deptno로 dname출력 - JSON, JSP, combobox

![](../../.gitbook/assets/1%20%2855%29.png)

```markup
<input id="cc1" class="easyui-combobox" data-options="
        valueField: 'DEPTNO',
        textField: 'DNAME',
        url: '../datagrid/dept.json',
        onSelect: function(rec){
            var url = '../getEmpList.jsp?deptno='+rec.DEPTNO;
        }">
```

* API를 살펴보자
* valueField는 실제로 선택된 값, textField는 보여지는 값이다. 
* 나중에 검색조건으로 사용하기위해 Jsp, Servlet파일로 넘어가는 값은 valueField의 값일 것이다.
* json문서를 만들어 data를 넣어준다.
* onSelect, 클릭시 이벤트를 구현할 수 있다. - 6번은 클릭시 jsp문서에게 쿼리스트링으로 값을 넘긴다. - jsp문서에서 request.getParameter로 해당 값을 받아 조건으로 사용할 수 있을 것이다.

{% page-ref page="step1-getemplist.jsp-json-jsp.md" %}

## empManagerA,C,D - combobox심화

### 생각해보기

![](../../.gitbook/assets/3%20%2836%29.png)

* 검색분류에서 항목을 클릭시 서버에 넘어가는 값는 valueField의 값이고, 화면에 출력되는 내용은 textField값 일 것이다.
* Atype은 html만, B타입은 html+JS, C타입은 JS로만 구현해서 비교해보자 - Btype은 재사용성, 독립성이 높아 유지보수 업무시에 유리하다. - Ctype은 화면 가독성이 떨어질 것이다.

### 시점

1. &lt;table&gt; dom구성 완료
2. JS가 해당 table에 접근하여 이벤트 구현
   1. &lt;head&gt;영역 : function A = 호출시, 행위 발생시
   2. &lt;body&gt;영역 : ready\(function A\) = 브라우저 로딩 완료 후 자동 진행

### API 활용하기

```markup
<input id="cb_search" class="easyui-combobox" data-options="
        					valueField: 'cols',	textField: 'label'">
```

* 위 코드로 easyui-combobox class를 갖는 input태그에 valueField와 textField를 지정한다.
* valueField는 실제로 넘어가는 값, textField는 화면에 보여지는 값

```markup
<script type="text/javascript">   
   $(document).ready(function (){	 
	  $("#cb_search").combobox({ //콤보박스 초기화 및 설정
		  data:[
			   {cols:'empno', label:'사원번호'}
			  ,{cols:'ename', label:'사원이름'}
			  ,{cols:'sal'  , label:'급여'}
		  ]
	  	 ,onSelect:function(rec){//보박스에서 선택했을떄 @param:object로서 row의 주소번지를 갖는다. row.empno, row.ename
	  		alert('당신이 선택한 검색조건은 '+rec.cols);
			var url = '../getEmpList.jsp?cols='+rec.cols;
	  	 }
	  });
</script>
```

* data와 이벤트는 따로 분리하여 구현하는 것이 좋겠다. = JS
* 브라우저 로딩 후 콤보박스에 기본 값을 직접 넣어주고 : data : \[  {필드명:'값', 필드명:'값, ...}, {....} ,...\]
* onSelect이벤트로 선택시 이벤트를 실행한다.
* onSelect가 실행하는 함수의 파라미터는 object로서, 해당 row의 주소번지를 갖는다. - row.empno, row.ename, ....
* 위 함수를 활용해 11번에서 jsp파일로 조건으로 사용될 변수를 쿼리스트링으로 넘긴다.

### 사용한 datagrid API

![API](../../.gitbook/assets/4%20%2830%29.png)

* 참고 : [http://jeasyui.com/tutorial/datagrid/datagrid12.php](http://jeasyui.com/tutorial/datagrid/datagrid12.php)

후기 : API보는 법을 발전 시키자 영어공부도 해야겠는데...

