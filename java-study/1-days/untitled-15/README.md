---
description: 2020.10.28 - 51일차
---

# 51 Days - 넥사크로설정1, visual studio code설정, eclipse-Git설정, date-number-textbox

### 사용 프로그램

* 사용언어 : JAVA(JDK)1.8.0\_261, JS, JQuery
* 사용Tool \
  \- Eclipse : Eclipse.org\
  \- Toad DBA Suite for Oracle 11.5\
  \- Visual Studio Code
* 사용 서버\
  \- WAS : Tomcat

## 복습

* **오라클**\
  \- Select를 시작으로 컬럼에 접근한다.
* **자바**(처리주체-서버)\
  \- 객체지향언어, 컴파일한다\
  \- List\<Map>에 n개의 값을 담는다.\
  \- List : index, Map : key
* **HTML**(처리주체-클라이언트)\
  \- 절차지향언어, 컴파일(문법체크)하지않는다.\
  \- 화면, 입력 값을 읽고 응답을 화면에 쓴다.\
  \- html : input, easyui : textbox\
  \- easyui.js와 같은 API를 사용하면 화면 생성시 별도의 태그를 생성해준다.\
  \- html** id는 JS**가 사용한다.** name은 서버(자바-servlet,jsp)**에서 사용한다.\
  \- name으로 JAVA와 소통한다.
* **JS**\
  \- 동적처리의 시작, html의 event담당\
  \- \<head>: 호출, \<body>:선언 후 -> 시점 설계가 중요하다.\
  \- \<head>는 시점에 호출하는 것이고, \<body>는 순서대로 실행된다.\
  \- \<body>에서 id에 접근시 선언 위에 코드를 작성하려면 onload(JS), ready(Jquery)사용\
  \- 페이지 이동시 파라미터로 data를 넘기고 응답을 리턴한다.

### server.xml

* 사용할 서버에 대한 설정 xml파일
* 커넥션 풀 설정
* 배치(import)
* 한글처리

## empManager - Dialog, NumberBox

### 작업지시서

* dataset으로 소통, 출력이 이루어져야한다. ui솔루션을 사용해 다양한 표현을 해보자
* emp.json(xml)의 dataSet(xml)을 사용해본다.
* JS기반의 ui 솔루션 : easyui(API), bootstrap(반응형 웹)
* empManager.html 로 페이지를 생성한다.\
  \- 2단계 : empMagager.jsp
* html에 테이블 생성\
  \- html : table, easyui : datagrid(Object로 제공)
* 바인딩\
  \- \<input class="easyui-datagrid" || "easyui-textbox"/>\
  \- easyui를 사용하면 개발자가 부여한 input태그의 id는 처음 로드되었을 때만 살아 있고, ready나 onload, event 등이 발생하면 easyui가 제공하는 id로 덮여 씌워진다.

### Dialog

* easyui제공 : [http://jeasyui.com/documentation/index.php#](http://jeasyui.com/documentation/index.php#)
* Dialog API를 확인하면서 코드를 작성한다.\
  \- closed:true(창 닫기), false(창 열기)\
  \- modal:true(dialog 뒷 화면 비활성화). false(활성화)

### datebox

* easyui제공 : [http://jeasyui.com/documentation/index.php#](http://jeasyui.com/documentation/index.php#)
* required옵션 : 

### \<label>

* html\
  \- \<label>이름\</label>\
    \<input type="text">
* easyui\
  \- \<input label="이름">

{% content-ref url="untitled.md" %}
[untitled.md](untitled.md)
{% endcontent-ref %}

## textboxTest, datagrid

### textbox

![17번 코드 적용](<../../../.gitbook/assets/4 (27).png>)

![아이콘 클릭 -> 18-24번 코드 적용](<../../../.gitbook/assets/5 (19).png>)

```markup
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>textboxText.html</title>
<!-- 공통코드 추가[css, jquery.js, easyui.js] -->
	<link rel="stylesheet" type="text/css" href="https://www.jeasyui.com/easyui/themes/default/easyui.css">
    <link rel="stylesheet" type="text/css" href="https://www.jeasyui.com/easyui/themes/icon.css">
    <link rel="stylesheet" type="text/css" href="https://www.jeasyui.com/easyui/themes/color.css">
    <link rel="stylesheet" type="text/css" href="https://www.jeasyui.com/easyui/demo/demo.css">
    <script type="text/javascript" src="https://www.jeasyui.com/easyui/jquery.min.js"></script>
    <script type="text/javascript" src="https://www.jeasyui.com/easyui/jquery.easyui.min.js"></script>
</head>
<body>
	<script type="text/javascript">
   		$(document).ready(function(){
	   		$("#tb_name").textbox('setValue', '테스트');
	   		$("#tb_name").textbox({	   			
	   			icons: [{
	   				iconCls:'icon-search',
	   				handler: function(e){//이벤트 발동시 진행
	   					$(e.data.target).textbox('setValue', 'Something added!');
	   				}
	   			}]
	   		});///////////end tb_name
 </script>
    
    <!-- text box -->
    <input id="tb_name" class="easyui-textbox">
</body>
</html>
```

* easyui제공 : [http://jeasyui.com/documentation/index.php#](http://jeasyui.com/documentation/index.php#)

{% content-ref url="textboxtest-deptview-textbox-datagrid.md" %}
[textboxtest-deptview-textbox-datagrid.md](textboxtest-deptview-textbox-datagrid.md)
{% endcontent-ref %}

## 넥사크로 설정1 - framework를 사용하지 않을때

### 넥사크로

* 자바 언어가 아닌 UI솔루션이므로 jsp, servlet을 사용해 자바와 연결한다.
* Servlet을 지원한다.\
  \- 자바코드, req, res객체 지원

### 설치 및 설정

![](<../../../.gitbook/assets/1 (50).png>)

1. 네이버 - 투비소프트 검색 - UI/UX - 넥사크로 플랫폼
2. 다운로드 - 압축 풀기
3. Eclipse에 dynamic project 생성\
   \- 위 이미지 창까지 진행시 체크박스 체크하기(설정파일 생성)
4. 넥사크로 파일에서 jar파일을 생성한 프로젝트의 WecContetn - WEB-INF - lib 폴더 안에 배포

## Visual Studio Code 설정

### visual Studio Code

* Eclipse같은 Tool
* 깃과 연동 가능
* 정적 작업 

### 설치 및 설정

![](<../../../.gitbook/assets/4 (25).png>)

1. 구글 - Visual Studio Code 검색
2. Window - System Installer - 64 bit 설치
3. 관리자 모드로 실행 

## Eclipse-Git 설정

### Preferences - 동기화 설정

![General - Workspace](<../../../.gitbook/assets/2 (38).png>)

![General - Workspace - Build](<../../../.gitbook/assets/3 (31).png>)

* visual studio code와 연동해서 사용할 때에도 필요하다. 자동 동기화 시켜준다.
* Eclipse : 동적 작업 

후기 : 홈트를 위해 요가매트랑 폼롤러를 주문했고 삼성전자 주식을 사버렷다!
