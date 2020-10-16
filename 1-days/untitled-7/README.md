---
description: 2020.10.16 - 43일차
---

# 43 Days - HTML구조, 태그, 플러그인, 이미지, 링크, &lt;span&gt;, &lt;div&gt;

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261 : Oracle.com
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5
* 사용 FrameWork - MyBatis

## 복습

### 웹 서비스 개요

* html : 화면, View계층 담당 - document가 html을 객체로 받아 변수를 사용할 수 있게 해준다.
* Java : 로직, Model계층 담당 - 화면과 로직은 각 담당자들이 만드므로 화면없이도 로직을 만들기 위해 클래스 쪼개기를 해야한다. - Java의 io 패키지는 화면 구현 기능을 지원한다
* 어떤 언어든 브라우저가 읽을 수 있다면 UI를 표현할 수 있다.

### 순서\(알고리즘\)

* 로그인 페이지에서 로그인 값을 기억한다. -&gt; 상품페이지로 이동한다.
* HTML이 말하고, JS가 기억하고 JAVA를 통해 DB로 연결된다.

## HTML

### HTML구조

![](../../.gitbook/assets/html.png)

```markup
<!DOCTYPE html>
<html>
 <head>
 <title>Page Title</title>
 </head>
 
 <body>
 <h1>This is a Heading</h1>
 <p>This is a paragraph.</p>
 </body>
</html>
```

* 1번 : HTML선언
* 2번 : 루트태그
* 3,7번 : HTML은 두 영역을 갖는다. - &lt;head&gt;영역. &lt;body&gt;영역
* &lt;head&gt;&lt;/head&gt; - 화면에 보여지지않는 영역 - CSS, Javascript가 올 수 있다. - 내부 html확장자, 외부확장자를 배치하는 영역
* &lt;body&gt;&lt;/body&gt; - 화면에 보여지는 영역 - MarkUp = 애니메이션, 이동이 구현되는 곳이다. - 태그들은 중복사용될 수 있다.
* 자바스크립트는 document객체로 HTML을 주입받아 사용한다.

### document

* JS에서 확장자가 html. htm인 문서 전체를 받는 최상위 객체
* 브라우저가 제공하는 내장객체로, 인스턴스화가 필요없다.
* File file = new File\(html\) 하는 것과 같이 html을 객체로 담는다.

### HTML특징

* 컴파일 하지 않는다. - 내용 변경시 컴파일 없이 화면을 새로고침하기만 해도 적용된다.
* 변수를 사용할 수 없다. - 제어문을 사용할 수 없고, 변환된 값을 읽어 가질 수 없다.
* 보이는 태그, 보이지 않는 태그를 갖는다.
* 페이지를 이동시켜주는 태그를 지원한다. ex\) &lt;a&gt;&lt;/a&gt;
* 단방향 서비스만 제공한다. 정적이다. - JS가 head영역에서 양방향 서비스, 동적구현을 지원해준다. - 듣기\(값 저장, 유지\)와 제어문을 사용 가능하게 해준다.

### 태그

![](../../.gitbook/assets/3%20%2815%29.png)

```markup
<body>
html
그럴까
<br>좋아요
<p>줄바꿈</p>
</body>
```

* 브라우저가 인터프리터방식으로 읽어 body안의 contents만 보여준다.
* 모든 태그들은 열린 순서대로 닫혀야한다.
* 태그는 적용할 내용을 감싼다.
* **블럭요소** : Layer로서 자체 크기를 갖기때문에 자동 줄바꿈된다.  - 대표적인 태그 : &lt;div&gt;, .....
* **인라인요소** : 자체 크기가 없어 자동줄바꿈이 일어나지 않는다. - 대표적인 태그 : &lt;a&gt;, &lt;span&gt;, &lt;button&gt;, &lt;image&gt;, .... - 줄바꿈 태그 &lt;br&gt;로 줄바꿈효과를 줄 수 있다.
* 상속관계가 있다.  - &lt;table&gt;의 &lt;th&gt;, &lt;tr&gt;, &lt;td&gt;태그와 같이 부모 태그의 영역에서만 사용가능한 태그가 있다.

### Attribute\(속성\)

* 좌변에 속성, 우변에 값이 온다.
* &lt;h1 style = "font-size : 60px;"&gt;적용할 내용&lt;/h1&gt; &lt;열린태그 속성=예약어:값&gt; 컨텐츠 &lt;닫는태그&gt;
* **ID 속성** - 주로 UI솔루션에서 많이 사용한다. - JS, 넥사크로, ....
* **name 속성** - 주로 Java의 서블릿에서 사용한다. - JS에서는 자바와 연결될때 사용한다.
* border속성으로  table의 테두리같은 선을 표시할 수 있다.

### style의 %, px

* 1024x768사이즈 화면이라면, 세로선 1024개, 가로선 786개가 만나는 지점들을 화소 라고한다. 만나는 부분이 잦을 수록 화소가 높아져 화질이 좋아진다.
* **%** - 비율로 크기를 정한다. - 디바이스, 배경마다 크기가 자동으로 변화한다.
* **px** - 고정값

### 플러그인

* 웹 브라우저의 표준기능을 확장해주는 프로그램 - Java Spplet, Flash Player, Pdf Reader, .....
* object요소나 embed요소를 사용해 HTML문서에 추가할 수 있다.

### 애플리케이션, 하이브리드

* 애플리케이션 : 혼자힘으로, 단독으로 실행되는 것
* 하이브리드 앱 : 브라우저를 이용한 애플리케이션 - html5 2.0부터 지원한다.

### UI/UX 솔루션

* 최근에 UI구현시 많이 사용되는 솔루션이다.
* 기술을 지원해 기간단축해주고, 생산성이 좋다. - 페이지 생산성이 좋아 기다림이 줄어든다.
* Javascript 기반 : open API를 이용해 기술지원은 없고 html에서만 사용가능하다.
* XML 기반 : 다른 디바이스에서도 화면구현에  사용할 수 있다. - 넥사크로 등

## **Javascript**

```markup
<body>
<!-- html 영역, java코드 작성 불가 -->
<!-- 사용하기전에 script선언 먼저 : 마임타입, 메인text서브javascript -->
<script type="text/javascript"> 
<!-- 자바스크립트 영역, 자바코드를 사용할 수 없다. -->
	document.write("나는<b><u>내장객체</u></b>를 지원하는 함수입니다.")
	document.write("<br>")
</script>
</body>
```

* &lt;head&gt;, &lt;body&gt;모든 영역에서 사용될 수 있다.
* 브라우저가 읽을수 있어 화면을 구현할 수 있지만 html보다 번거롭다. - 6,7번 처럼 html에 적는 document.write메서드를 매번 사용해야 하므로
* 4번 : 먼저 mime타입으로 선언이 이루어져야한다.
* 4-8번 : 스크립트 영역으로 선언된 곳에는 //주석을 쓸 수 있다.

## GSON, JSON

![&#xC624;&#xB77C;&#xD074;, &#xC790;&#xBC14;, html Table Data](../../.gitbook/assets/3%20%2816%29.png)

### oracle : table

* row+columns
* UI는 지원하지 않는다.

### java : table

* JTable + DefaultTableModel - 화면양식\(UI지원\) + JOSON\(JSON이 데이터를 지원한다.\)

### html : &lt;table&gt;

* &lt;th&gt; : 헤더, 가운데 정렬이 default 정렬 값, tableheader
* &lt;tr&gt; : row의 갯수, tableroe
* &lt;td&gt; : row를 컬럼별로 쪼개준다. align속성으로 정렬해야 정렬된다. tabledata
* 상속관계, dom tree구조 - &lt;table&gt;안에 &lt;th&gt;, &lt;th&gt;안에 &lt;tr&gt;, &lt;tr&gt;안에 &lt;td&gt;를 사용할 수 있다.
* DOM - Document Object Model - 브라우저가  dom tree구조를 제공한다.

## HTML 태그

### &lt;span&gt;

* 보여지지않는 태그
* CSS영역으로, 부분적 style적용이 일어날때 사용한다.

### &lt;div&gt;

* 보여지지않는 태그
* 박스
* AJAX에서 가장 사용빈도가 높은 태그이기도 하다.
* div를 조작하기 위해 ID를 부여해서 사용한다.

### &lt;a&gt;

```markup
<body>
<a href="http://localhost:9000/day1/hello.html">This is a link</a>
내용을 적으면 줄바꿈이 안된다.
<br>
인라인요소는 자체 크기가 없다.
</body>
```

* 페이지 이동 태그, hert속성에 URL을 지정해 줄 수 있다. - 화면에 해당 URL 링크를 출력한다.
* &lt;a&gt;태그는 인라인요소로, 자체 크기가 없다.
* 3번처럼 &lt;br&gt;태그 없이 텍스트를 작성하면 줄바꿈없이 옆에 이어서 출력된다.
* 아래 코드 처럼 &lt;a&gt;태그 사이의 버튼, 이미지등에 도 링크를 걸 수 있다.

### &lt;image&gt;

```markup
<body>
<a href="http://www.naver.com">
<img src="http://localhost:9000/images/babyApech.png"/>
</a>
</body>
```

* 이미지 파일 호출 태그, src속성에 URL로 불러올 수 있다.
* 3번은 eclipse에서의 이미지 경로 URL이다.
* 이 코드를 실행해 이미지를 클릭하면 네이버로 페이지가 이동한다.

후기 : 

