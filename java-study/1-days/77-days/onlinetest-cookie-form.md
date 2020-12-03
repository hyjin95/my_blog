# onLineTest : cookie와 form페이지 이동

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



