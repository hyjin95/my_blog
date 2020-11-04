---
description: 2020.11.04 - 56일차
---

# 56 Days - 개발패턴:Action-JSP

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5
* 사용 서버 - WAS : Tomcat

## 필기

### html과 jsp - 원본과 복사본

![](../.gitbook/assets/1%20%2857%29.png)

* session ID가 같으면, 브라우저는 화면에 끼워넣기를 한다. 단, html의 경우에는 메모리에 있는 것을 가져오므로 기존의 데이터를 가져온다.
* JSP는 이런 시점의 문제에서 비교적 자유롭다. JSP파일은 내부적으로 a.jsp -&gt; a.java -&gt; a.class순서로 컴파일 하므로 jsp파일이 수정되면 java, class파일도 새로 읽어 들인다.
* 서버와 동기화 되는 페이지를 만들때에는 JSP를 이용하는 것이 안전하다.

### JSON- stringify, split, substring, parse

![9&#xBC88; &#xB2E8;&#xC704;&#xD14C;&#xC2A4;&#xD2B8;](../.gitbook/assets/2%20%2845%29.png)

```javascript
function empSearch(){        	 
    $("#dg_emp").datagrid({            	
            url:'../../getEmpList2.jsp?cols='+$("#cb_search").val()+"&keyword="+$("#sb_keyword").val();
            ,onLoadSuccess:function(temp){            		
            var result = JSON.stringify(temp);   
   			   	//alert("onload....."+result);
            var test = result.split('"rows":',2);
            var test2 = test[1].substring(0,test[1].length-1);
   			 	  //alert("onload....."+test2); 
   			 	  var result2 = JSON.parse(test2);
   			 	  //alert("onload....."+result2[0].ENAME);    			 		
   			 }		
     });
 }
```

* HTML/JSP에서 JSON형식으로 보낸 data는 Object로 읽힌다. 이 data에 JS가 접근해 지정 data를 꺼내려면 Object덩어리를 분리해야한다.
* Sting화 -&gt; 분리 기준지정\(구분자\) -&gt; 필요한 String만 자르기 -&gt; JSON화
* **JSON.stringif**y : Object를 String타입으로 변환한다.
* **소유주.split\(구분자, 배열갯수\)** : String덩어리를 구분자를 사용해 나눠 배열로 담아준다. - 소유주 : 분리할 String data
* **소유주.substring\(여기부터, 여기까지\)** : 지정된 구간 내의 문자열을 잘라내 담는다. - 소유주 : 필요한 값이 들어있는 배열\(방\)
* **JSON.parse\(값\)** : 파라미터로 받은 data를 JSON형식으로 변환한다.

## empManagerFinal - JSP로 구현

## 개발패턴

### 1. JSP -&gt; JSP

* DB를 거치지 않는 작업.

### 2. JSP -&gt; Action -&gt; JSP

* 화면\(검색요청\) -&gt; DB검색 -&gt; 검색결과화면 - ex\) 수정 : 화면에 기본 조회를 깔아 놓고 놓아져있는 data를 골라서 수정한다. = JSP로 시작   DB를 경유하는 시작이 아니므로 cost가 적게 발생한다.

### 3. Action -&gt; JSP -&gt; Action -&gt; JSP

* 기본 DB전체조회 -&gt; 화면 -&gt; DB검색 -&gt; 검색결과화면 - ex\) 수정 : DB에서 기존 data를 가져와야 한다 = Action으로 시작   DB를 경유하는 시작이므로 시작부터 cost가 발생한다.

### 4. Action -&gt; Action -&gt; JSP

* DB입력 -&gt; DB조회 -&gt; 결과화면

