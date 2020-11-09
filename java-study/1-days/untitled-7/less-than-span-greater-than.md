# 인라인요소, &lt;span&gt;

## spanTest.html

![&#xC778;&#xB77C;&#xC778;&#xC694;&#xC18C;](../../../.gitbook/assets/1%20%2827%29.png)

```markup
<!DOCTYPE html>
<html>
<head>
 <meta charset="UTF-8">
 <title>Insert title here</title>
 <script type="text/javascript"> 
	//멤버변수, 함수선언, 호출해야 실행
	function methodA(){
		alert("methdA 호출성공");//alet로 출력하면 작은 화면이 나온다.
	}
 </script>
</head>

<body>
 여기<span id="sp_msg"></span>
 저기<span id="sp_msg2"></span>
 <!-- 자바는 객체지향언어로 메서드 선언위치가 변경되어도 접근이 가능하지만
      절차지향적 언어인 javaScript는 함수, 태그의 선언위치에 따라 접근이 불가능 할 수 있다.
    JS는 무조건 앞에서 선언되어야 접근이 가능하고 컴파일 하지않는 언어이므로 런타임시에 모든것이 결정된다. -->
 <script type="text/javascript"> 
	//지역변수, 즉시 실행
	document.getElementById("sp_msg").innerText="<strong>span태그는 보이지 않습니다.</strong>";
	document.getElementById("sp_msg2").innerHTML="<u>span태그는 보이지 않습니다.</u>";
	document.methodA();
 </script>
</body>
</html>
```

## tableTest.html

![&amp;lt;span&amp;gt;](../../../.gitbook/assets/2%20%2819%29.png)

```markup
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>테이블 태그 실습</title>
</head>
<body>

<h2>Basic HTML Table</h2>

<table border="3" style="width:100%">
<caption>사원목록</caption>
<colgroup>
	<col span="2" style="background:yellow"/>
    <col style="background:red"/>
</colgroup>
<thread style="background:green">
  <tr>
    <th colspan="3">Firstname</th>
    <th rowspan="3">Lastname</th> 
  </tr>
</thread>
 <tbody>
  <tr>
    <td>Jill</td>
    <td rowspan="2">Jill</td>
    <td>Smith</td>
  </tr>
  <tr>
    <td>Eve</td>
    <td>Jackson</td>
  </tr>
  <tr>
    <td>John</td>
    <td>Doe</td>
  </tr>
  </tbody>
  <tfoot style="background:gray">
  <td colspan="3" align="center">회사소개 | 회사연혁 | 오시는길</td>
  </tfoot>
</table>
</body>
</html>
```

