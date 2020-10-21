# &lt;table&gt; - datagrid level1-3

## level1 - table 생성

![table data&#xAC00; &#xC5C6;&#xB2E4;](../../.gitbook/assets/table-border.png)

```markup
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>level1 - 태그만으로 구성하기</title>
	<!-- 인터넷이 가능한 환경이므로 URL을 통해 import, link를 건다. -->
 	<link rel="stylesheet" type="text/css" href="https://www.jeasyui.com/easyui/themes/default/easyui.css">
    <link rel="stylesheet" type="text/css" href="https://www.jeasyui.com/easyui/themes/icon.css">
    <link rel="stylesheet" type="text/css" href="https://www.jeasyui.com/easyui/themes/color.css">
    <link rel="stylesheet" type="text/css" href="https://www.jeasyui.com/easyui/demo/demo.css">
    <script type="text/javascript" src="https://www.jeasyui.com/easyui/jquery.min.js"></script>
    <script type="text/javascript" src="https://www.jeasyui.com/easyui/jquery.easyui.min.js"></script>
</head>
<body>
	<table border="1" id="dg_dept"></table>
</body>
</html>
```

* data는 없어  테두리 선을 나타내는 border마저 없으면 화면에 아무것도 나타나지 않는다.
* 화면의 .은 border값에 의해 나타난 것

### level1 - html태그, JSON만으로 생성

![](../../.gitbook/assets/1%20%2833%29.png)

```markup
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>level1 - 태그만으로 구성하기</title>
	<!-- 인터넷이 가능한 환경이므로 URL을 통해 import, link를 건다. -->
 	<link rel="stylesheet" type="text/css" href="https://www.jeasyui.com/easyui/themes/default/easyui.css">
    <link rel="stylesheet" type="text/css" href="https://www.jeasyui.com/easyui/themes/icon.css">
    <link rel="stylesheet" type="text/css" href="https://www.jeasyui.com/easyui/themes/color.css">
    <link rel="stylesheet" type="text/css" href="https://www.jeasyui.com/easyui/demo/demo.css">
    <script type="text/javascript" src="https://www.jeasyui.com/easyui/jquery.min.js"></script>
    <script type="text/javascript" src="https://www.jeasyui.com/easyui/jquery.easyui.min.js"></script>
</head>
<body>
	<table data-options="url:'./dept.json'" id="dg_dept" class="easyui-datagrid">
		<thead>
        	<tr>
            	<th data-options="field:'DEPTNO',width:'35%', align:'center'">부서번호</th>
            	<th data-options="field:'DNAME',width:'35%', aligln:'center'">부서명</th>
            	<th data-options="field:'LOC',width:'30%',align:'center'">지역</th>
        	</tr>
    	</thead>
    </table>
</body>
</html>
```



