---
description: 2020.11.04 - 56일차
---

# 56 Days - 개발패턴:Action-JSP, UTF-8.java, &lt;a&gt;, JSON-stringify, split, parse, substring, empManagerF

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5
* 사용 서버 - WAS : Tomcat

## 필기

### html과 jsp - 원본과 복사본

![](../../../.gitbook/assets/1%20%2858%29.png)

* session ID가 같으면, 브라우저는 화면에 끼워넣기를 한다. 단, html의 경우에는 메모리에 있는 것을 가져오므로 기존의 데이터를 가져온다.
* JSP는 이런 시점의 문제에서 비교적 자유롭다. JSP파일은 내부적으로 a.jsp -&gt; a.java -&gt; a.class순서로 컴파일 하므로 jsp파일이 수정되면 java, class파일도 새로 읽어 들인다.
* 서버와 동기화 되는 페이지를 만들때에는 JSP를 이용하는 것이 안전하다.

### JSON- stringify, split, substring, parse

![9&#xBC88; &#xB2E8;&#xC704;&#xD14C;&#xC2A4;&#xD2B8;](../../../.gitbook/assets/2%20%2845%29.png)

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
   			 	  var jsonDoc = JSON.parse(test2);
   			 	  //alert("onload....."+result2[0].ENAME); 
     			 	for(var i=0;i>jsonDoc.length;i++){
	            alert("empno : "+jsonDoc[i].JOB)
            }	
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
* 12,13번 : 뽑아낸 n개의 row의 JOB컬럼 value를 출력해본다.
* 이러한 함수들 사용 없이는 StringTokenizer와 같은 함수로 일일히 자르거나 해야한다.

### onLoadSuccess - 이벤트핸들러\(인터셉트\)

* onLoadSuccess:function 함수이름 \(파라미터\){실행, 구현문} - 일회성 함수라면 함수이름은 생략할 수 있다. - 파라미터에는 jsp에서 out.print된  data가 들어온다.   JSP의 mimtype이 application/json이라면 data는 json형식인 Object타입의 data이다.
* datagrid에 사용하면 조회가 정상적으로 로드되었는지, 갱신 처리가 성공했는지 알 수 있다.

### &lt;a&gt; 태그

* href를 가질 수 있다.
* 역할 - 문단이동 : 스크롤바를 이동시킬 필요 없이 목차를 누르면 해당 목차로 바로 이동하는 것 - link - JS함수 호출

## UTF-8설정 - get, post전송

* 같은 설정이더라도 전송 방식에 따라서 결과가 다를 수 있다.

{% page-ref page="hangulconversion.java-utf-8-get-post.md" %}

## empManagerFtype - checkbox, dialog, Enable DataGrid Inline Editing

### API활용 - checkbox,dialog, Enable DataGrid Inline Editing

1. 사용할 위치 확인
2. script부분과 화면 부분 분리
3. JS에서 접근할 id 확인

### checkbox 추가

![](../../../.gitbook/assets/1%20%2857%29.png)

*  API : [http://jeasyui.com/documentation/index.php\#](http://jeasyui.com/documentation/index.php#) - Form -&gt; checkbox - default : 0 = 체크되지 않은 상태
* 체크박스를 추가할 datagrid가 load되는 시점에 체크박스 컬럼을 추가한다.
* 모든 컬럼을 조회하는 함수가 있다면 해당 함수의 컬럼에도 추가한다.
* 조회하는 SQL문에 체크박스 컬럼을 추가한다.

### dialog 추가 : 수정

*  API : [http://jeasyui.com/documentation/index.php\#](http://jeasyui.com/documentation/index.php#) - Window -&gt; dialog
* dialog로 띄울 div에 calss='easyui-dialog'를 부여한다.
* 수정하려는 목적의 dialog는 입력값을 DB에 전송해야 할 것이기 때문에 &lt;form&gt;태그로 감싼다.
* 수정하려는 항목 중, 범위가 정해져 있는 항목은 DB에서 값을 가져와 combobox로 나타내준다.

### Enable DataGrid Inline Editing

* 참고 : [http://jeasyui.com/tutorial/datagrid/datagrid12.php](http://jeasyui.com/tutorial/datagrid/datagrid12.php) - 하단의 zip을 다운받아 코드를 풀어보면 더 자세하게 살펴볼 수 있다.
* 수정, 삭제, 저장 등을 지원해준다.

{% page-ref page="empmanagerftype.jsp-checkbox-dialog-edit.md" %}

## 개발패턴

### 1. JSP -&gt; JSP

* DB를 거치지 않는 작업.

### 2. JSP -&gt; Action -&gt; JSP

* 화면\(검색요청\) -&gt; DB검색 -&gt; 검색결과화면 - ex\) 수정 : 화면에 기본 조회를 깔아 놓고 놓아져있는 data를 골라서 수정한다. = JSP로 시작   DB를 경유하는 시작이 아니므로 cost가 적게 발생한다.

### 3. Action -&gt; JSP -&gt; Action -&gt; JSP

* 기본 DB전체조회 -&gt; 화면 -&gt; DB검색 -&gt; 검색결과화면 - ex\) 수정 : DB에서 기존 data를 가져와야 한다 = Action으로 시작   DB를 경유하는 시작이므로 시작부터 cost가 발생한다.

### 4. Action -&gt; Action -&gt; JSP

* DB입력 -&gt; DB조회 -&gt; 결과화면

후기 : 영하1도, 미국 대선 ㄷㄷㄷㄷ

