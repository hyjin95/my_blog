---
description: 2020.11.17 - 65일차
---

# 65 Days -

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5
* 사용 서버 - WAS : Tomcat

## 복습

### JSP의 mime type

1. HTML
2. JSON
3. XMl : nexacro

### MVC패턴

* view와 logic을 구분해서 작업할 수 있는 개발방식

### 서블릿 관리

* 배치 서술자 파일에 url-pattern을 등록해 WAS, 프레임워크가 운영과 라이프사이클을 관리하게 한다.
* LifeCycle을 이렇게 외부에서 관리받는 것을 역제어 라고 한다.
* 외부에서 관리를 받으려면 배치서술자 파일에 url을 등록하는 것이 필수조건이다.

### JSP, Servlet, Web.xml 경로 맞추기

* Jsp문서가 a폴더 밑에 있다면, 서블릿도 src밑에 a패키지 밑에 만들고, 배치 서술자 파일의 url-pattern등록시에도 /a/\*.do 이렇게 경로를 맞춘다.

## boardSell.jsp : 제공

### 주어진 코드 : View

![](../../../.gitbook/assets/boardsold-.png)

```markup
<h2>보드 판매량</h2>
<table width="300px" height="80px">
   <tr>
      <th width="120px">보드 판매량</th>
      <td width="180px"><span id="boardSold">10</span></td>
   </tr>
   <!-- 
   소비자가-구매가=보드 한개당 마진 금액
   한개당 마진 금액*판매량=마진금액계산
    -->
   <tr>
      <th>구매가</th>
      <td><span id="cost">100</span>원</td>
   </tr>
   <tr>
      <th>소비자가</th>
      <td><span id="price">120</span>원</td>
   </tr>
</table>
<h2>마진금액 : <span id="cash">0</span>원</h2>
<button onclick="getBoardSold()">마진은?</button>
```

* body부분 코드

```css
 body {
      font-family: sans-serif;
      text-align: center;   }
   table {
       margin-left: auto;
       margin-right: auto;
      border: 1px solid black;   }
   td,th {
      border: 1px dotted gray;
      text-align:center;   }
   th {
      background-color: #FFAAAA;   }
```

* CSS style코드

### 주어진 코드 : API - getText\( \)

```javascript
   //span태그가 가지고 있는 텍스트 노드값을 읽어오기
   function getText(el){
      var text="";
      if(el!=null){
         if(el.childNodes){
            for(var i=0;i<el.childNodes.length;i++){
               var childNode = el.childNodes[i];
               //너 텍스트 노드니?
               if(childNode.nodeValue !=null){
                  text = text+childNode.nodeValue;
               }      
            }
         }
      }
      return text;
   }
```

* 2번 getText\(el\) : el=태그의 주소번지
* 5번 if\(el.childNodes\) : 자식노드가 있으면 true, 없으면 false - el.childNodes = 태그의 자식노드 = text Node
* 9번 if\(childeNode.nodeValue!=null\) : 자식노드가 null이 아니라면
* 이런 data를 가져올때에는 항상 null인경우를 대비해야한다. if문을 사용하자.
* 반복적으로 text노드에 접근해야하므로 재사용가능한 메서드로 만든다.

### 주어진 코드 : API - replaceText\( \)

```javascript
   //기존 TextNode의 값을 다른 문자열로 바꾸기
   /***********************************************
   param1 :document.getElementById("boardSold")
   param2 :xhrObject. 
   ************************************************/
   function replaceText(el, value){
      if(el !=null){
         clearText(el);//기존에 있던 10을 지워주세요.
         //새로운 텍스트 노드 15를 생성하기
         var newNode = document.createTextNode(value);//15
         //위에서 생성한 텍스트 노드 값을 el에 붙이는 함수 호출하기
         el.appendChild(newNode);
      }
   }
```

### 주어진 코드 : API - clearText\( \)

```javascript
   //기존 태그안에 문자열 지우는 함수 구현
   function clearText(el){
      if(el !=null){
         if(el.childNodes){
            for(var i=0;i<el.childNodes.length;i++){
               var childNode = el.childNodes[i];
               el.removeChild(childNode);
            }
         }
      }
   }
```

## boardSell.jsp : Level1 - JS로 textNode접근하기

### 작업지시서

1. 마진금액 = \(소비자가 - 구매가\) \* 보드 판매량
2. 구매가를 가져오는 함수를 활용해 그 값을 변수에 담는다. 같은 방식으로 필요한 데이터를 가져온다.
3. 2번에서 가져온 값으로 보드의 개당 마진금액을 계산해본다.
4. 총 마진금액을 구해 화면에 출력한다.

### textNode에 접근하기

* textNode의 값을 가져와 사용해야한다.
* textNode는 nodeName을 갖지않고, nodeValue만을 가지기 때문에 id를 가질 수 없다.
* 외적 변화가 없는 태그&lt;span&gt;을 사용해 감싸 id를 부여해 접근한다. - div도 외적변화가 없지만 자체크기를 갖는 블록요소이기때문에 &lt;span&gt;을 사용한다.
* Id에 접근해 해당 id를 가진 node의 주소번지 가져오기 - JS : document.getElement\("id"\) - JQuery : $\("\#id"\)

### getBoardSold\( \)메서드 정의

* "마진은?" 버튼을 클릭할때 호출되는 함수
* &lt;span&gt;태그의 id에 접근해 주소번지를 담는 변수를 선언하고,  해당 변수를 getText메서드의 파라미터로 넘겨 text를 가져온다.
* consol.log\( \)메서드로 가져온 값을 담은 변수를 출력해 단위테스트 할 수 있다. - 브라우저의 개발자도구의 console 탭에 출력된다.
* 꺼내온 값들을 사용해 필요한 데이터를 출력한다. 

### UI에 마진값 보여주기

* 값을 성공적으로 꺼내왔다면, replaceText메서드를 활용해 값을 UI에 출력해준다.

### JQuery로 바꿔보기

* JS는 표준이지만, JQuery는 표준이 아니기 때문에 JS에서 자유롭게 사용하던 표준 API들을 사용하지 못하는 경우도 있으니 주의하자.

{% page-ref page="boardsell.jsp-level1-js-textnode.md" %}

## boardSell.jsp : Level2 - JSP, JS로 비동기통신 구현하기

* JQuery의 ajax를 이용한 비동기 통신이 아닌 JS만을 이용해 수동 비동기 통신을 구현해본다. = DOM부분처리

### 작업지시서

1. '마진은?' 버튼이 눌리면 서버에 접속해 집계한다. - 동기화를 위해 서버를 경유한다. - 동기화 하기 위해 request + url\(통신\) 이 필요하다.   url을 통해 요청을 하는데, 요청을 하려면 채널이 필요하다.
2. 재사용성을 위해 JSP가 아닌 Servlet을 사용한다.
3. 통신객체 XMLHttpRequest\( \)를 생성해 이 객체로 통신한다. - 이 작업을 ajax는 ajax가 해준다. - 객체이기때문에 함수, 속성을 사용할 수 있다.   xhrObject = new XMLHttpRequest\( \);   xhrObject.onreadystatechange=sold\_process; - 상태 체크 속성

### 채널 사용하기

* 배웠던 방법 : socket
* 오늘 사용할 방법 : XMLHttpRequest\( \)
* 아마존, 스프링클라우드 등 시스템, 디바이스 마다 채널을 제공하는 방법이 다르다. - 그 안에서 공통되는 부분을 활용해야한다.

### 통신상태 : xhrObject.readyState

* xhrObject = new XMLHttpRequest\( \);
* 0\(uninitialized\) : open메서드가 호출되기 전
* 1\(loading\) : HTTP요청 준비가 된 상태
* 2\(loaded\) : HTTP요청을 보내 처리하는 중 - 헤더는 읽을 수 있는 상태   get방식인지 post방식인지, http프로토콜의 버전, 브라우저 타입을 확인할 수 있다.
* 3\(interactive\) : 데이터를 받는중, 하지만 완전히 받지는 못한 상태
* 4\(complete\) : 데이터까지 완전히 받은 상태 - 비로소 responseText나 혹은 responseXML속성으로 데이터를 읽을 수 있는 상태

{% page-ref page="boardsell.jsp-level2-servlet-js.md" %}

## boardSell.jsp : Level2\_2 - 서블릿으로 구현하기

{% page-ref page="boardsell.jsp-level2\_2.md" %}



