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
	//여기서 DB와 연동해 로그인 인증처리를 해야한다.
	response.sendRedirect("./onLineTestOk.html");
%>
</body>
</html>
```

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
<script type="text/javascript">
/*********************************************************************
 * 체크박스의 경우 멀티 선택을 지정하는 속성이 제공되지 않으므로 사용자가 만들어서 제공해야 한다. 
 * 체크박스에 onChange이벤트를 추가해 체크박세에 변화가 발생되면 브라우저가 감지하게 되고
 * 그 때 test함수가 호출된다.
 * 이 때 파라미터로 0,1,2,3을 받아서 그 값을 cb배열의 인덱스 값과 같은지를 비교한다.
 * 만약 같다면 체크박스에 체크를해주고 나머지는 0을 대입해 체크가 해제되도록한다.
 *
 * @param pcb 사용자가 선택한 체크박스의 index값이 저장된다.
 ********************************************************************/
	function test(pcb){//변수 pcb에는 0,1,2,3중 하나씩 값이 들어온다.
		for(var i=0;i<document.f_test1.cb.length;i++){
			if(pcb==i){
				document.f_test1.cb[i].checked=1;
			}else{
				document.f_test1.cb[i].checked=0;
			}
		}
	}////////////////end of test
	
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

### 코드 : testForm2.jsp

```markup
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
	String test1 = request.getParameter("htest1");
	out.print("1번 문제 답안 - "+test1);
	Cookie c = new Cookie("test1", test1);
	c.setMaxAge(60*60);//1시간
	response.addCookie(c);//이 코드를 작성하지 않으면 쿠키는 저장되지 않는다.
%>
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
</form>
<br>
<input type="button" value="이전문제" onClick="prev()"/>
<input type="button" value="제출" onClick="next()"/>
</body>
</html>
```

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
	String daps[] = {"3", "4", "1", "2", "4"};
	String tests[] = new String[5];
	Cookie[] cs = request.getCookies();
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



