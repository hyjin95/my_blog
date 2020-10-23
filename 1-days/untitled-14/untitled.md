# easyui활용했을 떄 AJAX가 JSP를 거치면 - basic

### basic.html

```markup
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>basic.html-:easyui-textbox</title>
 	<link rel="stylesheet" type="text/css" href="https://www.jeasyui.com/easyui/themes/default/easyui.css">
    <link rel="stylesheet" type="text/css" href="https://www.jeasyui.com/easyui/themes/icon.css">
    <link rel="stylesheet" type="text/css" href="https://www.jeasyui.com/easyui/themes/color.css">
    <link rel="stylesheet" type="text/css" href="https://www.jeasyui.com/easyui/demo/demo.css">
    <script type="text/javascript" src="https://www.jeasyui.com/easyui/jquery.min.js"></script>
    <script type="text/javascript" src="https://www.jeasyui.com/easyui/jquery.easyui.min.js"></script>
    <script type="text/javascript">
    	//get방식이라면 서버에 데이터가 전달되지 않을 수 있다. -> 쿼리스트링을 이용하거나 새로운 session을 만들어 값을 유지해야한다.
       //함수로 돌려야함
    	function test(){
			$("#d_msg").text($("#emp_id").val()+", mem_id : "+$("#mem_id").val());
    	}
    	function test2(){
    		$.ajax({
    			url:'basicAction.jsp'
    		   ,success:function(data){
    			   alert("data : "+data);
    			   //$("#mem_id").val(data);//jsp파일을 거치면서(다른페이지를 거치면) 불능이 됨
    			   $("#_easyui_textbox_input1").val(data);//다른 페이지를 거치더라도 정상 작동
    			   $("#emp_id").val(data);
    		   },error:function(error){//에러가 발생했을때 실행되는 이벤트
    			   alert(error.responseText);
    		   }
    		});
    	}
    </script>
</head>
<body>
<script type="text/javascript">
	$(document).ready(function(){
		//document.getElemnetById("emp_id").value//JS표준
		//text();(Jquery) : text로 찍어주세요=innerText(표준) html(Jquery)이라면 html으로 찍어주세요=innerHTML(표준)
	});
</script>
<!-- 아이디는 인라인 요소이지만 화면에서는 input 밑에 나타난다. easyui가 코드를 추가 작성하기 때문 -->
<!-- iconCls를 바꿀 수 있다 두번째 import link가 연관되어있을것이다! -->
아이디1 : <input class="easyui-textbox" id=mem_id" n"ame="mem_id" data-options="iconCls:'icon-man'" style="width:200px">
<hr>
아이디2 : <input id="emp_id" name="emp_id" style="width:200px" value="tomato">
<button onclick="test()">전송</button>
<button onclick="test2()">전송2</button>
<!-- alret으로 출력하는 테스트가 제일 간단하지만 div연습을 위해 div를 사용해보자 -->
<div id="d_msg"></div>
</body>
</html>
```

### basicAction.jsp

```markup
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
이상해
```

## 실행

![&#xCF54;&#xB4DC;&#xB97C; &#xBCF4;&#xBA74; easyui &#xC544;&#xC774;&#xB514;&#xB97C; &#xBCFC; &#xC218; &#xC788;&#xB2E4;.](../../.gitbook/assets/1%20%2845%29.png)

![&#xAE30;&#xBCF8; &#xC804;&#xC1A1;&#xBC84;&#xD2BC;](../../.gitbook/assets/2%20%2835%29.png)

![&#xC804;&#xC1A1;2 alert](../../.gitbook/assets/3%20%2828%29.png)

![&#xACB0;&#xACFC;](../../.gitbook/assets/4%20%2821%29.png)

