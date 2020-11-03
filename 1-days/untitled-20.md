---
description: 2020.11.02 - 55일차
---

# 55 Days -

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5
* 사용 서버 - WAS : Tomcat

## 복습

### combobox, WHERE - AND, OR, 비교연산자

```javascript
$("#cb_search").combobox({
		  data:[
			   {cols:'empno', label:'사원번호'}
			  ,{cols:'ename', label:'사원이름'}
			  ,{cols:'sal'  , label:'급여'}
		  ]
	  	 ,onSelect:function(rec){//콤보박스선택 -> @param:object로서 row의 주소번지를 가짐. row.empno, row.ename
	  		alert('당신이 선택한 검색조건은 '+rec.cols);
			var url = '../getEmpList.jsp?cols='+rec.cols;
	  	 }
	  });
```

* AND : 교집합, 원소가 감소한다.
* OR    : 합집합, 원소가 증가한다. - 데이터가 방대해질 수 있어 잘 사용하지 않는다. 시간이 오래걸릴 수 있다.
* WHERE 컬럼명 비교연산자 값
* 위 코드에서는 WHERE cols = 값 이런식으로 사용할 수 있을 것이다. - cols는 사용자에 따라 변하는 data
* 콤보박스 옆에 textbox를 만들어 사용자에게서 비교할 대상이 되는 값을 받아와야 할 것이다. - WHERE cols = textbox입력값
* combobox에서 실제로 넘어가는 값은 valueField, 사용자에게 보여지는 값은 textField이다. - 넘어오는 data는 실제로 DB에 있는 컬럼명이여야 하고, 사용자에게는 text형태로 정의된 직관적인 값이여야 하기 때문이다.

### 병합

* 가로병합 : colspan = 병합할 셀 수
* 세로병합 : rowspan = 병합할 셀 수



