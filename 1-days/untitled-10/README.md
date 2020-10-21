---
description: 2020.10.20 45 일차
---

# 45 Days - URI, html-JS Event,

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261 : Oracle.com
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5
* 사용 라이브러리 - JQuery

## 복습

### HTML tree구조

* HTML파일을 브라우저가 DOMtree형식으로 변환한다. - 모든 태그\(노드\)들은 id와 name으로 식별한다.
* &lt;head&gt; - 선언\(&lt;link&gt;=import\) - 404, undefine에러가 발생하는 부분 - CSS, JS작성 가능 - 화면에 보여지지 않는다. - 에러를 확인하기 힘드므로 URL 단위테스트를 통해 확인한다.
* &lt;body&gt; - &lt;table&gt; : dataSet\(java의 list, Map\) - &lt;tr&gt;과 &lt;td&gt;로 table의 면을 만들어 정보를 담을 수 있게한다. - 이렇게 만들어진 table은 양식, form이다. - &lt;tr&gt; : map, VO, &lt;td&gt; : 원시타입 변수

### HTML JSP, UI/UX솔루션

* html은 정적이다.\(반복되는 코드를 하나로 만들어 코드를 줄일 수 없다.\) - 처리 주체가 **브라우저**인 정적 페이지로 동기화를 제공하지 않는다.
* 이런 정적인 특징을 보완하기위해 JSP를 사용한다. - Java Server Page, 처리주체가 **서버**인 자바 기반 동적 페이지
* 페이시 생산성을 위해서는 UI/UX솔루션을 사용한다. - JS기반의 easyUI, xml기반의 넥사크로 등

### Tag name, id

* 해당 태그에 접근하기 위한 식별자
* name으로 접근하기 위해서는 부모태그들을 거쳐야한다.
* 이런 접근의 문제를 JQuery를 통해 보완하자.
* id 접근 : getElementById\(" "\)
* name 접근 : getElementByName\(" "\)
* 또는 접근할 태그, 노드를 &lt;form&gt;으로 감싼다. - &lt;form&gt;태그는 일괄 전송이 가능하다. - 여러개 전송에 유리하다.
* 또는 URL 쿼리스트링으로 접근한다. - xxx.html?id=test&pw=123 - 노출된다. - 주소를 가지고 링크를 걸 수 있다.
* ID : , JS영역이나 JSP의 데이터 처리 영역에서만 사용된다.
* name : JSP는 물론 컨트롤러에서도 파라미터로 사용할 수 있다.
* JSP로 만들어진 model에서 데이터를 처리하고, 응답을 View에서 화면에 처리해야하는데 이때 필요한 중간 통신다리가 컨트롤러이다.

### 전송방식

1. **get** 방식 - http프로토콜을 이용해 주소를 가지고 링크를 걸 수 있다. - 포장이 안되어 노출 된다. - URL+append하는 것이므로 주소창의 길이 한계에 부딪히는 제약이 있다. - 주소창의 크기는 브라우저에 따라 다르다.  - 데이터 조회용
2. **post**방식 - 링크를 걸 수 없어 노출이 없다. - 데이터 수정용 - get방식보다 대용량의 전송을 할 수 있다.

### http프로토콜 전송

* 클라이언트가 서버에 요청\(request\)하면 서버가 요청에 대해 응답\(response\)하는 구조 - 사용자로부터 http프로토콜을 사용해 서버에 전송한다.
* http프로토콜 : 비 상태 프로토콜
* stateless, 연결을 계속 유지하는 것이 아니라 응답을 받으면 바로 물리적인 연결을 끊어버린다. 필요할때마다 다시 연결한다.
* 많은 동시 접속자 수를 관리하기 용이하다.
* &lt;form&gt;과 같은 전송 방법으로 파라미터를 같이 전송 할 수 있다.

### 데이터의 입력과 전송

* UI에서 사용자가 입력한 값을 표준JS나 JQuery API를 이용해 서버에 전송한다. - document.getElementById\(" "\), $\("\# "\)
* 서버에서도 전송된 값을 읽기 위해서는 JAVA가 필요하다. - JSP에서 request.getParameter\(" "\)로 파라미터를 읽어온다.
* 서버가 DB에 저장하고, htm, html 마커 언어를 통해 화면에 응답한다.

### HTML - JSP, JSON

* JSON으로 dataSet 구현 - 동기화 되지 않은 상태 - 현재 값이 아닌 과거 값이 나올 수 있다.
* JSP로 dataSet 구현 - 동기화 지원
* html스스로는 dataSet을 가질 수 없다.

### JS

* html문서를 동적으로 태그를 추가 할 수 있다. - document.createElement\("table"\)
* 이런식의 동적 작동이 가능하므로 JS를 기반으로 하는 오픈소스 API가 생겨날 수 있다. - easyUI 등
* 문자열은 ", '를 사용해야 문자열로 인식한다.

## 필기

### URI

![](../../.gitbook/assets/image%20%281%29.png)

* Uniform Resource Identifier
* 통합 자원 식별자
* 인터넷에 있는 자원을 가르키는 유일한, 유니크한 주소
* 브라우저가 요구하는 가장 기본 조건으로, http프로토콜과 항상 붙어 다닌다.
* URl, URN을 포함하는 개념
* http://test.com/test.pdf?docid=111 이라는 주소가 있다면, - URI에는 포함되지만 URL에는 포함되지 않는다. - http://test.com/test.pdf까지만 URL이다.
* 유일한 식별자까지를 포함한다. - http://test.com/test.pdf?docid=111  - http://test.com/test.pdf?docid=111  - 위 두 주소는 같은 URL을 갖지만 URI는 서로 다르다.

### URL

* Uniform Resource Locator
* 파일 소스 경로의 일부이면서 자원의 개념
* 서버의 핸들러로 사용됨으로서 자원이라고 한다.
* 웹 서비스를 제공하는 각 서버에 있는 파일의 위치를 표시한다.

### URN

* Uniform Resource Name
* 위치정보와는 상관없이 실제 리소스의 이름값으로만 접근하는 방식

### MVC 패턴

* Model-View-Controller
* 소프트웨어 디자인 패턴, 개발시 3가지 형태로 역할을 나누어 개발하는 방법론
* **비즈니스 처리 로직과 UI요소들을 분리시켜 서로 영향없이 개발하기 수월**하다. - 화면없이 로직을 구현할 수 있다.
* **Model** - 애플리케이션의 정보, data, 알고리즘, DB와의 상호작용 등 을 관리 - 애플리케이션이 "무엇"을 할 것인지 정의, 내부 비즈니스 로직을 처리하기 위한 역할 - 모델의 상태에 변화가 있을떄 컨트롤러와 뷰에 통보 - 이를 통해 뷰는 최신 결과를 보여주고 컨트롤러는 변화에 따른 명령을 추가,수정,제거한다.
* **View** - 사용자 인터페이스, UI요소 - UI를 위해 모델의 data를 얻어온다. - 한 모델이 여러개의 뷰를 가질 수 있다.
* **Controller** - 데이터와 비즈니스 로직 사이의 상호동작 관리 - 모델이 "어떻게"처리할지를 알려주는 역할 - 모바일에서는 화면의 로직처리도 담당한다. - 화면에서 사용자 요청을 받아 처리되는 부분을 구현, 요청내용을 분석해 모델, 뷰에 업데이트 요청 - 중개인 역할 - 모델의 상태를 변경하는 명령을 모델에 내릴 수 있다.\(워드프로세서에서 문서편집\) - 해당 뷰에 모델의 표시 방법을 변경하는 명령을 내릴 수 있다.\(문서를 스크롤하는 것\)
* 복잡한 대규모 프로그램을 개발할때에는 한계가 있다. 다수의 뷰와 모델이 컨트롤러를 통해 복잡하게 연결되면 수정, 파악이 어려워지는데, 이를 대규모 MVC 애플리케이션, Massive ViewController라고 한다.

## 데이터 소통

### InputReadTest.html, Action.jsp 설계

![&#xC124;&#xACC4;&#xB3C4;](../../.gitbook/assets/1%20%2832%29.png)

* JSP파일에서 넘어온 응답이 화면에 출력될때에는 기존의 html파일이 아닌 새로운 파일에서 작성된다.

## JS Event

### JS, JAVA Event

<table>
  <thead>
    <tr>
      <th style="text-align:center">JS Event</th>
      <th style="text-align:center">JAVA Event</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:center">event Handler &#xC81C;&#xACF5;</td>
      <td style="text-align:center">&#xC774;&#xBCA4;&#xD2B8; &#xC778;&#xD130;&#xD398;&#xC774;&#xC2A4; &#xC81C;&#xACF5;</td>
    </tr>
    <tr>
      <td style="text-align:center">
        <p>onClick</p>
        <p>onKeyup</p>
        <p>onKeydown</p>
        <p>.....</p>
      </td>
      <td style="text-align:center">
        <p>ActionListener</p>
        <p>MouseListener</p>
        <p>WindowListener</p>
        <p>.....</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:center">&#xB300;&#xC785; &#xC5F0;&#xC0B0;&#xC790;&#xB85C; &#xD568;&#xC218;&#xD638;&#xCD9C;&#xD558;&#xC5EC;
        &#xC815;&#xC758;</td>
      <td style="text-align:center">&#xC81C;&#xACF5;&#xB41C; &#xCD94;&#xC0C1;&#xBA54;&#xC11C;&#xB4DC;&#xB85C;
        &#xC774;&#xBCA4;&#xD2B8; &#xC815;&#xC758;</td>
    </tr>
  </tbody>
</table>

### JS Event

1. 이벤트를 적용할 태그에 id로 접근해야한다. - document.getElementById\(" "\)로 접근
2. DOM tree가 완성 되어야 접근이 가능하다. - onload 이벤트핸들러를 사용하거나 이벤트가 body의 선언 뒤에 위치해야한다. - 브라우저가 HTML파일을 DOM구성으로 다운로드 완료해야 접근할 수 있으므로
3. JS에서 onload이벤트를 function\( \){ } 익명함수로 정의한다. - 단, 익명함수로 정의하면 재사용될 수 없다. - window.onload = function\( \) { }
4. 이벤트를 html문서의 head영역에 onload조건으로 구현한다. - 단위테스트 : function\( \) {alert\("호출성공"\)}

### eventBubbling

```markup
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>이벤트 버블링 방식</title>
<style type="text/css">
	div {border : 3px solid black;}
	* {border : 3px solid black;}
</style>
</head>
<body>
<!-- 이벤트 버블링은 자식노드에서 부모노드 순으로 이벤트를 실행하는 것을 말한다. onclick=null 코드로 방지할 수 있다.-->
	<div onclick="alert('outter-div이벤트 버블링')">
		<div onclick="alert('inner-div이벤트 버블링')">
			<h1 onclick="alert('header 이벤트 버블링')">
				<p onclick="alert('이벤트 버블링')">이벤트 전이</p>
			</h1>
		</div>
	</div>
</body>
</html>
```

* 브라우저가 이벤트를 감지하는 방식중 하나
* 특정 화면 요소에서 이벤트 발생시 해당 이벤트가 더 상위의 화면 요소들에게 전달되어지는 특성 - 브라우저는 중첩된 요소에서 이벤트 발생시 그 이벤트를 최상위 화면 요소까지 전파한다. - 위 코드를 실행하면 나오는 결과와 같다.
* 방지하기 - event를 null로 초기화하기 - evnet.stopPropagarion\( \)사용하기

### 문자열에 이벤트 구현하기

```markup
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1 id="head">클릭해주세요</h1>
	<script type="text/javascript">
		var head = document.getElementById("head");//object로 인식
		
		//이벤트 연결
		head.onclick = function(){
			alert("클릭 호출 성공");
			//만약 이벤트를 한 번 처리 하고나면 실행되지 않게 하고 싶다면
			head.onclick = null;
		}		
	</script>
</body>
</html>
```

* 8번 : 문자열에 id부여
* 9번 : JS선언
* 10번 : Element의 ID를 받는 변수 선언
* 13-17번 : 이벤트 부여 - 16번 : 이벤트 처리를 막아 놓는 구문
* &lt;h1&gt;태그에 싸인 문자열 중, 부분 문자에 이벤트를 적용하려면 &lt;span&gt;태그로 감싸 id를 따로 줘야한다.
* 부분 문자열에만 이벤트를 부여해보자

{% page-ref page="js.md" %}

### onkeyup 이벤트핸들러 구현하기

* onkeyup : 키보드 키가 눌렸다가 위로 올라왔을때
* input태그로 만든 입력창에 이벤트를 구현해보자

{% page-ref page="onkeyup.md" %}

### onsubmit 이벤트핸들러 구현하기

| 이벤트 핸들러 | 이벤트 소스 |
| :---: | :---: |
| onsubmit - 전송 | 정보를 서버에 전달하는 용도의 버튼 등 |

* html문서를 Object객체로 담아주는 document객체로 html 태그에 접근할 수 있다. - document.getElementById\("이벤트소스이름"\) - 이벤트소스 = 가입버튼, 전송버튼 등...
* **이벤트소스이름.onsubmit\(처리될 파라미터\);**
* 토글버튼 : 한번 버튼이 눌려지고 나면 초기화 전까지는 비활성화된다. - 이런 경우에는 함수를 재사용할 필요 없으므로 익명함수로 정의한다. - ~ = fucntion\( \){ 구현 - 변수초기화, if, for, ... }; --재사용하지 않으므로 ; 으로종료 - java에서의 내부클래스와 비슷하다.   btn\_send.addActionListener\(new ActionListener\( \){   public void actionPerformed\(ActionEvent e\){ 구현문}\);
* 가입 버튼을 누르면 값을 비교하는 JS를 작성해보자

{% page-ref page="onsubmit.md" %}

후기 : JS에 JAVA에 JQuery에 JSP 등등 자바 하나만 하다가 다른 여러개체들을 만나니 개념공부를 더 해야겠다는 생각이 들었다.

