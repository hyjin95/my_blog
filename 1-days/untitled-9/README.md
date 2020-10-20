---
description: 2020.10.19 - 44일차
---

# 44 Days - innerText, innerHTML, JS현재시간, onload, tag이해하기, DOM, JQuery, node, AJAX

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261 : Oracle.com
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5
* 사용 FrameWork - MyBatis

## 복습

### &lt;span&gt;, &lt;div&gt;

* 공통정 : 외적으로 보여지지 않는다.
* &lt;span&gt; - 인라인 요소\(자체 크기X, 옆에 붙는다.\) - 디자인적요소, 스타일 부분처리 시 사용
* &lt;div&gt; - block 요소\(자체 크기O, 아래에 붙는다.\) - button이벤트인 keyDown, onKeyUP등 에 사용된다.

### innerText, innerHTML

* innerText - 해당 영역을 모두 text형태로 인식한다. - 태그를 인식 할 수 없다.
* innerHTML - 해당 영역을 모두 html형태로 인식한다. - 태그 인식 가능
* AJAX에서 자주 사용된다.

### CSS

* 스타일 영역, 혼자서는 사용 의미가 없다.
* JS, HTML의 태그 안에서 지정 내용에 스타일을 준다.
* 스타일 부분처리, 전체 일관처리가 가능한 문서 디자인
* 전체 처리를 하기 위해 **css는 외부 문서로 작성**해 사용한다.

### html

![](../../.gitbook/assets/1%20%2830%29.png)

* dom tree의 노드들은 RAM에 저장되어 상주하므로 서버를 거칠 필요 없이 get방식으로 꺼낼 수 있지만, 이 방식은 동기화는 이루어지지 않는다.
* head영역 - title, JS, CSS, ...
* body영역 - 보이는 영역, JS, CSS, TAG등 모두 사용가능
* JS는 태그에게 이벤트를, CSS는 태그에게 스타일을 제공한다.

### HTML, JS table값 접근

* HTML - 브라우저가 배열로 바꿔주거나 ID를 이용해 table의 row값에 접근한다.
* JS - Element로 태그에 접근한다. - document.getElementById\( \);

## 필기

###  JQuery\(JS\)

```markup
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<!-- min=minimun, 쓸데없는 여백 등을 없애 좀더 가볍게 함 -->
<script type="text/javascript" src="https://code.jquery.com/jquery-3.5.1.min.js"></script>
</head>
<body>
테스트
<div id="d_msg"></div>
<div id="d_msg2"></div>
<script type="text/javascript">
	var dmsg = document.getElementById("d_msg");//js
	dmsg.innerText = "아이디를 입력하세요.";
	var dmsg2 = $("#d_msg2");////////////////////jquery
	dmsg2.text("비밀번호를 입력하세요.");
</script>
</body>
</html>
```

* JS기반 오픈 소스 라이브러리, API로서 w3cDom의 표준은 아니지만 거의 표준처럼 사용된다. - AJAX에서 많이 사용
* **DOM이 변환해준 HTML문서 객체에 보다 쉽게 접근하기 위한 JS의 라이브러리** - JS로 html에 접근하기 위해서는 document.getElementById\(" "\); 코드가 길다. - JQuery로 접근하면 $\("\# "\)로 끝나버린다.
* API를 이용해 객체를 태그 안에 넣어 사용한다. - 객체 = 함수+변수 - JS, JQuery에서 지원되는 객체들을 **HTML에서는 태그안에서** 기능을 사용할 수 있다.
* **크로스 브라우징을 지원**한다. - 크로스 브라우징 : 화면이 여러 브라우저, 크롬, 파이어폭스, 사파리, 익스플로러 등에서 모두 같은 형태를 유지하는것. - 개발자는 모든 브라우저에서 동일한 서비스를 제공해야한다.  - JQuery는 크로스 브라우징 서비스를 제공한다.
* JS로는 HTML문서를 만들 수 있지만 HTML태그 만으로는 JS를 구성할 수 없다.
* JS JQuery를 이용해 html 태그에 접근한다. - JS를 이용해 DOM트리로 만든 것 = easyUI, bootstrap - html과 Js모두를 조작할 수 있는 UI/UX솔루션

### HTML onload 이벤트

* onload객체가 로드 되었을떄 발생하는 이벤트 - **HTML문서의 모든 &lt;body&gt;영역이 웹페이지에 로드 완료 되었을때 스트립트를 실행**한다. - 브라우저가 서버로부터 해당 html문서를 전부 다운로드 받았을때
* JAVA의 EventHandler와 비슷한 역할
* 생김새 - &lt;body onload="적용할 기능이나 함수"&gt; - JS안에서도 window.onload를 이용해 지정할 수 있다.
* 한 HTML문서 안에서 단 한번만 사용 가능하다.  - 여러개 정의 되어있는 경우에는 제일 마지막 onload를 실행

{% page-ref page="untitled.md" %}

### &lt;form&gt;태그

* 웹 페이지에서의 **입력 양식**
* 입력된 데이터를 한 번에 서버로 전송한다.  - 전송된 데이터를 웹 서버가 처리하고 결과에 따라 다른 웹 페이지를 보여주기도한다.
* 속성 - action : 폼을 전송할 서버쪽 스크립트 파일 지정 - name : 폼을 식별하기 위한 이름 지정 - accept-charset : 폼 전송에 사용할 문자 인코딩 지정 - target : action에서 지정한 스크립트 파일을 현재 창이 아닌 다른 창에 열도록 지정 - method : 폼을 서버에 전송할 http메서드를 정한다.                    get또는 post방식
* form태그는 전체 양식으로, 화면에 보이지않는 추상적 태그이다. - 실제로 사용자가 양식을 입력하려면 &lt;input&gt;태그 등을 사용해야한다.

### &lt;input&gt;태그

* 사용자가 직접 값을 입력하게 해주는 태그
* 인라인 요소
* 속성 - type : 종류 지정              text, password, button, submit\(양식제출용버튼\), reset, radio\(단일선택 컴포넌트\), checkbox\(다중선택 컴포넌트\), file\(파일 업로드\), hidden\(사용자에게 보이지 않음\) - name - value : 기본 값

### error

* 404, undefined오류 : &lt;link&gt;에 오류가 있을 수 있다. - &lt;link&gt; = JAVA의 import역할
* 500번대 오류는 JAVA오류

## DOM

### DOM조작연습

* HTML + JS + JQuery만으로 화면 그리기
* step 1 : html - dataSet을 가질 수 없다.
* step 2 : html+js - 이벤트 처리를 할 수 있다.
* step 3 : js - JS로만 구성된 화면\(라이브러리 : bootstrap, 프레임워크 : easyUI\) - 순수 JS로만 작성하면 코드가 길어진다. - 소스분석이 어려워진다.
* step 4 : html+js+jquery

### DOM

* Document Object Model
* BOM중의 하나로, window객체의 하위 객체
* **HTML의 태그들을 JS가 접근할 수 있도록 문서 객체로 인식**해주는 document객체 지원 집합 - **웹 브라우저 내장객체**가 HTML페이지를 인식하는 방식중 하나라고 할 수 있다. - 원본 HTML문서의 객체 기반 표현 방식 - 단순 문서 파일인 HTML을 내용과 구조를 객체 모델로 변환해 다양한 프로그램에서 사용하게 한다.
* **tree\(돔 트리, 노드 트리\)형식의 자료구조**를 갖는다. - 하나의 root node에서 시작된다. - root node는 부모노드를 갖지않고 자식 노드만을 갖는다. - 자식이 없는 요소 안의 컨텐츠 노드는 leafg node라 한다. - tree는 RAM에 저장된다.
* DOM은 유효한 HTML문서의 인터페이스로, DOM을 생성하는 동안에 브라우저가 HTML코드를 교정해준다.
* html문서 파싱 -&gt; 태그를 DOM 노드로 변환 -&gt; 외부 css파일과 스타일 요소 파싱 -&gt; 또 다른 트리 생성
* DOM을 조작하는 객체 - ex\) img태그   &lt;img src="http://~" /&gt; - http://..... 은 절대경로이다.

### Node

![html node](../../.gitbook/assets/img_js_htmldom.png)

* HTML **DOM은 노드들의 집합인 노트 트리라는 계층적 구조에 저장**된다. - tree구조에서 root 노드를 포함한 모든 개체를 node라고 표현한다.
* **노드 : 계층 단위**, html정보가 저장된다.
* HTML DOM은 노드를 정의하고 관계를 설명하는 역할
* 모든 노드는 상속관계와 같이 계층적 관계를 맺는다.
* root 노드를 제외한 모든 노드는 단 하나의 부모 노드만을 갖는다.

### W3C HTML DOM표준

<table>
  <thead>
    <tr>
      <th style="text-align:center">&#xB178;&#xB4DC;</th>
      <th style="text-align:center">&#xC124;&#xBA85;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:center">&#xBB38;&#xC11C; &#xB178;&#xB4DC;(document node)</td>
      <td style="text-align:center">HTML&#xBB38;&#xC11C; &#xC804;&#xCCB4;&#xB97C; &#xB098;&#xD0C0;&#xB0B4;&#xB294;
        &#xB178;&#xB4DC;</td>
    </tr>
    <tr>
      <td style="text-align:center">&#xC694;&#xC18C; &#xB178;&#xB4DC;(elemnet node)</td>
      <td style="text-align:center">&#xBAA8;&#xB4E0; HTML&#xC694;&#xC18C;, &#xC18D;&#xC131; &#xB178;&#xB4DC;&#xB97C;
        &#xAC16;&#xB294; &#xC720;&#xC77C;&#xD55C; &#xB178;&#xB4DC;</td>
    </tr>
    <tr>
      <td style="text-align:center">&#xC18D;&#xC131; &#xB178;&#xB4DC;(attribute node)</td>
      <td style="text-align:center">
        <p>&#xBAA8;&#xB4E0; HTML&#xC694;&#xC18C;&#xC758; &#xC18D;&#xC131;, &#xC694;&#xC18C;&#xB178;&#xB4DC;&#xC758;
          &#xC815;&#xBCF4;&#xB97C; &#xAC16;&#xB294;&#xB2E4;.</p>
        <p>&#xD574;&#xB2F9; &#xC694;&#xC18C; &#xB178;&#xB4DC;&#xC758; &#xC790;&#xC2DD;
          &#xB178;&#xB4DC;&#xC5D0;&#xB294; &#xD3EC;&#xD568;&#xB418;&#xC9C0; &#xC54A;&#xB294;&#xB2E4;.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:center">&#xD14D;&#xC2A4;&#xD2B8; &#xB178;&#xB4DC;(text node)</td>
      <td style="text-align:center">HTML &#xBB38;&#xC11C;&#xC758; &#xBAA8;&#xB4E0; &#xD14D;&#xC2A4;&#xD2B8;</td>
    </tr>
    <tr>
      <td style="text-align:center">&#xC8FC;&#xC11D; &#xB178;&#xB4DC;(comment node)</td>
      <td style="text-align:center">HTML&#xBB38;&#xC11C;&#xC758; &#xBAA8;&#xB4E0; &#xC8FC;&#xC11D;</td>
    </tr>
  </tbody>
</table>

* HTML문서의 모든 것은 노드이다.

### BOM

* Browser Object Model
* 웹 브라우저와 관련된 객체들의 집합
* 최상위 객체 : window

### 웹 브라우저

* 모든 웹 브라우저는 스크립트가 접근할 수 있는 웹페이지를 만들기 위해 어느정도 dom을 허용한다.
* 스크립트를 작설하기 위해 인라인 &lt;script&gt;요소를 사용한다.
* html문서를 파싱하고 태그로 DOM트리를 구성한다.
* 웹 window = 자바 JFrame - open\( \)함수를 이용해 띄운다
* main메서드와 같은 실행 객체없이 URL로 요청을 받으면 활동한다. - main메서드로 만들수 있는 것은 local서버이다.
* 문서 파싱, 다운로드 -&gt; DOM구성

### DOM, JS

```markup
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script type="text/javascript">
	function test(){//head안에 잇으므로 불러야만 실행
		document.write("<u>밑줄글씨</u>")
		document.write("<table border='1' width='300px' height='150px'>");
		for(var i=0;i<5;i++){
		 document.write("<tr>");//테이블 생성시마다 document.write( );해야함 번거롭다. for문을 사용하게 해주는 유일한 장점
		 document.write("<td>"+i+"</td>");
		 document.write("</tr>");			
		}
		document.write("</table>");
	}
</script>
</head>
<body>
	<b>굵은글씨</b><!-- <b>는 인라인 요소 -->
	<script type="text/javascript">
	test();
		//alert("111");body태그에 스크립트 코드는 따로 호출하지 않더라도 자동으로 실행된다.
	</script>
</body>
</html>
```

{% page-ref page="untitled.md" %}

## onload를 이용한 현재시간 출력

### 준비

1. 어디에 출력할건지 위치 정하기 - 큰 글씨로 나타내기위해 &lt;h1&gt;태그를 이용해 body영역에 작성
2. 위치에 접근해야한다. - &lt;h1&gt;태그에 id속성을 추가한다.
3. 현재 시간을 계속 갱신해주는 함수가 필요하다. - JS가 제공해주는 setInterval\(적용할 기능, 시간간격\)함수를 사용한다. - 함수, 실행문을 정해진 시간 간격마다 반복 수행한다. - 적용할 기능 : 실행문, 함수\(재사용가능\) -&gt; { }를 사용할 수 있다.   시간간격 : ms - &lt;script type="text/javascript&gt;    self.setInterval\('timer\( \)', 100\);    function timer\( \){ document.myForm.myTimer.value = new Date\( \);}   &lt;/script&gt; --기본형 - self=this
4. onload이벤트를 사용한다.
5. 만들어진 HTML문서는 DOM방식으로 브라우저가 인식한다.

{% page-ref page="untitled-10.md" %}

## onClick, &lt;form&gt;을 이용한 구구단 구현

1. body영역에서 버튼에 onclick이벤트를 추가한다. - 스크립트 영역에서 접근하기 위해 id를 부여한다.
2. 웹 페이지 입력양식인 form태그를 이용해 서버에 전송할 영역을 제공한다.
3. form영역안에서 사용자의 입력값을 받기위한 input태그를 생성한다. - type : text - 사용자에게  시작할 단과 종료 단을 둘다 입력받아 서버에 전송하기 위해 name을 준다.

{% page-ref page="untitled-1.md" %}

후기 : 

