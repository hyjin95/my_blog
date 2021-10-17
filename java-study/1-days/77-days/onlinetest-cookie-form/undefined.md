# 코드

### 코드 : onLineTest.html

```markup
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>온라인 시험[onLineTest.html]</title>
<script type="text/javascript">
	function conform(){
		document.f_test.submit();
	}
</script>
</head>
<body>
<!-- 수험번호와 이름을 입력한 후 확인 버튼을 누른다.
	  페이지 이동처리 : onLineTestConform.jsp(인증처리, model1) -->
<table align="center" border="1" width="900px" height="600">
   <tr>
      <td valign="middle">
<!-- 수험생 검증 화면 시작  -->      
   <form name="f_test" method="get" action="./onLineTestConform.jsp">
      <table width="250px" height="90px" align="center">
         <tr><td height="25px">수험번호 : <input type="text" name="test_no" size="15"></td></tr>
         <tr><td height="25px">이&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;름 : <input type="text" name="test_name" size="15"></td></tr>
         <tr><td height="30px"align="center">
            <input type="button" value="확인" onClick="conform()">
          </td></tr>
      </table>
   </form>   
<!-- 수험생 검증 화면  끝   -->         
      </td>
   </tr>
</table>	  
</body>
</html>
```

### 코드 : onLineTestConform.jsp

```markup
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>인증처리 페이지[onLineTestConform.jsp]</title>
</head>
<body>
<%
	response.sendRedirect("./onLineTestOk.html");
%>
</body>
</html>
```

* 이 페이지에서 DB와 연동해 로그인 인증처리를 해야한다.
* 지금은 임시로 바로 인증완료 페이지로 페이지이동한다.

### 코드 : onLineTestOk.html

```markup
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>인증 성공시[onLineTestOk.html]</title>
</head>
<body>
<script type="text/javascript">
	alert("시험을 시작합니다.");
	location.href="testForm1.html";
</script>
</body>
</html>
```

### 코드 : testForm1.html

```markup
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>문제1[testForm1.html]</title>
```

* 첫번째 문제 페이지에서는 쿠키를 저장할 것이 없기때문에 jsp가 아닌 html로 구현했다.

```javascript
<script type="text/javascript">
	function test(pcb){//변수 pcb에는 0,1,2,3중 하나씩 값이 들어온다.
		for(var i=0;i<document.f_test1.cb.length;i++){
			if(pcb==i){
				document.f_test1.cb[i].checked=1;
			}else{
				document.f_test1.cb[i].checked=0;
			}
		}
	}////////////////end of test
```

* @param pcb : 사용자가 선택한 체크박스의 index값이 저장된다.
* 체크박스를 사용자가 선택하면 해당체크박스는 체크 처리되고, 나머지 체크박스들은 체크해제처리가 된다.

```javascript
	function next(){
		var temp = 1;//정답은 1번부터 이므로 1로 초기화한다.
		for(var i=0;i<document.f_test1.cb.length;i++){
			if(document.f_test1.cb[i].checked==1){
				document.f_test1.htest1.value=temp;
			}else{
				temp = temp + 1;
			}
		}
		document.f_test1.submit();//다음문제로 이동 --form전송으로 페이지 이동
	}///////////////end of next
</script>
```

* 다음문제 버튼 클릭시 사용자가 선택한 답을 hidden태그에 저장하는 함수이다.
* 3-8번 : 1번 체크박스가 체크되었다면 hidden태그의 값은 1이고 아니면 2번체크박스를 확인한다.\
  이렇게 체크박스 갯수만큼 반복하다가 체크된 체크박스가 있으면 해당 i값이 담긴다.
* 10번에서 form태그를 활용한 페이지 이동이 발생한다.\
  form태그 안의 action으로 페이지이동한다.

```markup
</head>
<body>
<form name="f_test1" method="get" action="testForm2.jsp">
<input type="hidden" name="htest1"/>
문제1. 다음 중 DML구문이 아닌 것을 고르시오.<br>
	<input type="checkbox" name="cb" onChange="test(0)">select<br>
	<input type="checkbox" name="cb" onChange="test(1)">insert<br>
	<input type="checkbox" name="cb" onChange="test(2)">create<br>
	<input type="checkbox" name="cb" onChange="test(3)">delete<br>
	<br>
	<input type="button" value="다음문제" onClick="next()"/>
</form>
</body>
</html>
```

* 체크박스의 경우 멀티 선택을 지정하는 속성이 제공되지 않으므로 사용자가 만들어서 제공해야 한다.
* 체크박스에 onChange이벤트를 추가해 체크박세에 변화가 발생되면 브라우저가 감지하게 되고 그 때 test함수가 호출된다.
* 이 때 파라미터로 0,1,2,3을 받아서 그 값을 cb배열의 인덱스 값과 같은지를 비교한다.\
  만약 같다면 체크박스에 체크를해주고 나머지는 0을 대입해 체크가 해제되도록한다.

### 코드 : testForm2.jsp

```java
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
	String test1 = request.getParameter("htest1");
	out.print("1번 문제 답안 - "+test1);
	Cookie c = new Cookie("test1", test1);
	c.setMaxAge(60*60);//1시간
	response.addCookie(c);//이 코드를 작성하지 않으면 쿠키는 저장되지 않는다.
%>
```

* 두번쨰 문제 페이지부터는 이전 페이지에서 넘어온 선택값을 Java로 쿠키에 담아 저장해야 하므로 파일형식을 jsp로 했다.
* 이전 페이지에서 get방식으로 페이지이동을 했기때문에 4번코드로 그 값을 가져올 수 있다.
* 6번에서 쿠키를 생성하고, 7번에서 유지시간을 지정하고, 8번에서 서버에게 쿠키를 내려보낸다.

```markup
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>문제2[testForm2.jsp]</title>
<script type="text/javascript">
	function test(pcb){
		for(var i=0;i<document.f_test2.cb.length;i++){
			if(pcb==i){
				document.f_test2.cb[i].checked=1;
			}else{
				document.f_test2.cb[i].checked=0;
			}
		}
	}
	
	function next(){
		var temp = 1;//정답은 1번부터 이므로 1로 초기화한다.
		for(var i=0;i<document.f_test2.cb.length;i++){
			if(document.f_test2.cb[i].checked==1){
				document.f_test2.htest2.value=temp;
			}else{
				temp = temp + 1;
			}
		}
		document.f_test2.submit();
	}
	function prev(){
		location.href="testForm1.html";
	}
</script>
</head>
<body>
<form name="f_test2" method="get" action="testForm3.jsp">
<input type="hidden" name="htest2"/>
문제2. 다음 중 DDL구문이 아닌 것을 고르시오.<br>
<input type="checkbox" name="cb" onChange="test(0)">create<br>
<input type="checkbox" name="cb" onChange="test(1)">drop<br>
<input type="checkbox" name="cb" onChange="test(2)">alter<br>
<input type="checkbox" name="cb" onChange="test(3)">delete<br>
<br>
	<input type="button" value="이전문제" onClick="prev()"/>
	<input type="button" value="다음문제" onClick="next()"/>
</form>
</body>
</html>
```

### 코드 : testForm3.jsp

```markup
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
 <%
	String test2 = request.getParameter("htest2");
	out.print("2번 문제 답안 - "+test2);
	Cookie c = new Cookie("test2", test2);
	c.setMaxAge(60*60);//1시간
	response.addCookie(c);//이 코드를 작성하지 않으면 쿠키는 저장되지 않는다.
%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>문제3[testForm3.jsp]</title>
<script type="text/javascript">
	function test(pcb){
		for(var i=0;i<document.f_test3.cb.length;i++){
			if(pcb==i){
				document.f_test3.cb[i].checked=1;
			}else{
				document.f_test3.cb[i].checked=0;
			}
		}
	}
	
	function next(){
		var temp = 1;//정답은 1번부터 이므로 1로 초기화한다.
		for(var i=0;i<document.f_test3.cb.length;i++){
			if(document.f_test3.cb[i].checked==1){
				document.f_test3.htest3.value=temp;
			}else{
				temp = temp + 1;
			}
		}
		document.f_test3.submit();
	}
	function prev(){
		location.href="testForm2.jsp";
	}
</script>
</head>
<body>
<form name="f_test3" method="get" action="testForm4.jsp">
<input type="hidden" name="htest3"/>
문제3. 다음 중 DCL구문으로 맞는 것을 고르시오.<br>
<input type="checkbox" name="cb" onChange="test(0)">grant<br>
<input type="checkbox" name="cb" onChange="test(1)">drop<br>
<input type="checkbox" name="cb" onChange="test(2)">alter<br>
<input type="checkbox" name="cb" onChange="test(3)">delete<br>
<br>
<input type="button" value="이전문제" onClick="prev()"/>
<input type="button" value="다음문제" onClick="next()"/>
</form>
</body>
</html>
```

### 코드 : testForm4.jsp

```markup
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
	String test3 = request.getParameter("htest3");
	out.print("3번 문제 답안 - "+test3);
	Cookie c = new Cookie("test3", test3);
	c.setMaxAge(60*60);//1시간
	response.addCookie(c);//이 코드를 작성하지 않으면 쿠키는 저장되지 않는다.
%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>문제4[testForm4.jsp]</title>
<script type="text/javascript">
	function test(pcb){
		for(var i=0;i<document.f_test4.cb.length;i++){
			if(pcb==i){
				document.f_test4.cb[i].checked=1;
			}else{
				document.f_test4.cb[i].checked=0;
			}
		}
	}
	
	function next(){
		var temp = 1;//정답은 1번부터 이므로 1로 초기화한다.
		for(var i=0;i<document.f_test4.cb.length;i++){
			if(document.f_test4.cb[i].checked==1){
				document.f_test4.htest4.value=temp;
			}else{
				temp = temp + 1;
			}
		}
		document.f_test4.submit();
	}
	function prev(){
		location.href="testForm3.jsp";
	}
</script>
</head>
<body>
<form name="f_test4" method="get" action="testForm5.jsp">
<input type="hidden" name="htest4"/>
문제4. 다음 중 테이블에 대한 설명으로 틀린 것을 고르시오.<br>
<input type="checkbox" name="cb" onChange="test(0)">
row와 column으로 구성되어있다.<br>
<input type="checkbox" name="cb" onChange="test(1)">
테이블에는 반드시 index가 있어야 한다.<br>
<input type="checkbox" name="cb" onChange="test(2)">
컬럼에는 적당한 타입을 선택하고 담을 수 있는 크기도 설정할 수 있다.<br>
<input type="checkbox" name="cb" onChange="test(3)">
테이블에는 PK도 올 수 있고 FK도 올 수 있다.
<br>
<input type="button" value="이전문제" onClick="prev()"/>
<input type="button" value="다음문제" onClick="next()"/>
</form>
</body>
</html>
```

### 코드 : testForm5.jsp

```markup
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
	String test4 = request.getParameter("htest4");
	out.print("4번 문제 답안 - "+test4);
	Cookie c = new Cookie("test4", test4);
	c.setMaxAge(60*60);//1시간
	response.addCookie(c);//이 코드를 작성하지 않으면 쿠키는 저장되지 않는다.
%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>문제5[testForm5.jsp]</title>
<script type="text/javascript">
	function test(pcb){
		for(var i=0;i<document.f_test5.cb.length;i++){
			if(pcb==i){
				document.f_test5.cb[i].checked=1;
			}else{
				document.f_test5.cb[i].checked=0;
			}
		}
	}
	
	function next(){
		var temp = 1;//정답은 1번부터 이므로 1로 초기화한다.
		for(var i=0;i<document.f_test5.cb.length;i++){
			if(document.f_test5.cb[i].checked==1){
				document.f_test5.htest5.value=temp;
			}else{
				temp = temp + 1;
			}
		}
		document.f_test5.submit();
	}
	function prev(){
		location.href="testForm4.jsp";
	}
</script>
</head>
<body>
<form name="f_test5" method="get" action="testForm6.jsp">
<input type="hidden" name="htest5"/>
문제5. 다음 중 프로시저에 대한 설명으로 틀린 것을 고르시오.<br>
<input type="checkbox" name="cb" onChange="test(0)">
프로시저를 생성할 때 파라미터를 선언할 수 있다.<br>
<input type="checkbox" name="cb" onChange="test(1)">
파라미터에 여러 변수를 선언할 수 있다.<br>
<input type="checkbox" name="cb" onChange="test(2)">
프로시저안에서 SELECT,INSERT,UPDATE, DELETE 모두 사용 할 수 있다.<br>
<input type="checkbox" name="cb" onChange="test(3)">
프로시저 안에서 commit할 수 없다.
<br>
<input type="button" value="이전문제" onClick="prev()"/>
<input type="button" value="제출" onClick="next()"/>
</form>
</body>
</html>
```

* 마지막 문제에서 생각해야하는 것은 마지막 문제의 선택값을 쿠키로 생성하는 페이지가 필요하다는 것이다.
* 제출버튼을 클릭시 testForm6.jsp로 페이지이동한다.

### 코드 : testForm6.jsp

```java
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
   String test5 = request.getParameter("htest5");
   Cookie c = new Cookie("test5",test5);
   c.setMaxAge(60*60);
   response.addCookie(c);
   response.sendRedirect("./testFormSend.jsp");
%>
```

* 이 페이지에는 html코드는 필요하지 않다. 
* 쿠키생성에 대한 코드와 채점관련 페이지로의 이동이 일어나는 코드가 필요하다.

### 코드 : testFormSend.jsp

```java
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
	//채점하는 페이지 --원래는 Oracle과 연동해 처리한다.

	//채점에 필요한 변수
	//1문제당 배점을 담을 변수
	int perJumsu = 20;
	//맞힌 갯수 카운트
	int cnt = 0;
	//판정하기
	boolean isPass = false;//true면 합격, false면 불합격
	//합격점
	int pass = 60;
	String daps[] = {"3", "4", "1", "2", "4"};//정답
	String tests[] = new String[5];//어느문제에대한 답인지 순서대로 담기위한 배
	Cookie[] cs = request.getCookies();//사용자가 입력한 정답들
```

* 채점에 필요한 변수를 선언한 부분이다.

```java
	if(cs!=null && cs.length>0){//쿠키가 존재하니?
			for(int i=0;i<cs.length;i++){
				String c_name = cs[i].getName();//test1, test2, test3, test4, test5, JSESSIONID
				if("test1".equals(c_name)){//1번답이니?
					tests[0] = cs[0].getValue();
				}
				else if("test2".equals(c_name)){//2번답이니?
					tests[1] = cs[1].getValue();
				}
				else if("test3".equals(c_name)){
					tests[2] = cs[2].getValue();
				}
				else if("test4".equals(c_name)){
					tests[3] = cs[3].getValue();
				}
				else if("test5".equals(c_name)){
					tests[4] = cs[4].getValue();
				}
			}//////////end of for
	}//////////////end of if
	
	//수험생이 선택한 답안지 정보 출력하기
	for(String dap : tests){
		out.print(dap+" -> ");
	}
```

* tests라는 빈 배열에 문제의 순서대로 쿠키의 값을 담는다.	

```java
	//정답을 몇개나 맞췄니?
	for(int i=0;i<daps.length;i++){
		for(int j=0;j<daps.length;j++){
			if(daps[i].equals(tests[j])){
				if(i==j){//자리까지도 일치하니?
					cnt++;
				}
			}
		}
	}
%>
```

* 정답배열과 tests배열의 방번호, 값 모두가 동일해야 정답처리한다.

```markup
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>여기서 채점해봐요.[testFormSend.jsp]</title>
</head>
<body>
맞힌 갯수 : <%=cnt %><br>
당신의 점수는 <%=perJumsu*cnt %>입니다.<br>
<%
	if((perJumsu*cnt)>=pass){
		isPass = true;
	}else{
		isPass = false;
	}
	response.sendRedirect("report.jsp?isPass="+isPass);
%>
</body>
</html>
```

* 바로 합격여부가 정해져 report.jsp로 페이지 이동이 일어나기때문에 8,9번 코드를 볼 수는 없지만, 16번코드를 주석처리하면 확인할 수 있다.

### 코드 : report.jsp

```java
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
   
<%
	String isPass = request.getParameter("isPass");
	boolean b_pass = Boolean.parseBoolean(isPass);
	if(b_pass){
		out.print("축하드립니다. 당신은 합격하셨습니다.");
	}else{
		out.print("당신은 불합격하셨습니다.");
	}
%>
```

