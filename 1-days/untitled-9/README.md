---
description: 2020.10.19 - 44일차
---

# 44 Days - innerText, innerHTML, JS현재시간, onload, tag이해하기,

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261 : Oracle.com
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5
* 사용 FrameWork - MyBatis

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

