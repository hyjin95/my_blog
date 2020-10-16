# dept - GSON,JSON

## 준비

* GSON다운 JSON.jar추가해야한다.

## SqlMapDeptDao.java

### main 메서드

```java
public static void main(String[] args) {
		SqlMapDeptDao dDao = new SqlMapDeptDao();
		List<Map<String, Object>> deptList = dDao.getDeptList();
		Gson g = new Gson();
		String temp = g.toJson(deptList);
		System.out.println(temp);
```

### 출력 data

```markup
[{"LOC":"NEW YORK","DEPTNO":10,"DNAME":"ACCOUNTING"},{"LOC":"DALLAS","DEPTNO":20,"DNAME":"RESEARCH"},{"LOC":"CHICAGO","DEPTNO":30,"DNAME":"SALES"},{"LOC":"포천시","DEPTNO":41,"DNAME":"개발2팀"},{"LOC":"서울","DEPTNO":99,"DNAME":"개발부"},{"LOC":"대전","DEPTNO":50,"DNAME":"개발부"},{"LOC":"광주","DEPTNO":60,"DNAME":"개발부"},{"LOC":"울산","DEPTNO":70,"DNAME":"개발부"}]
```

## dev\_HTML

### deptView.html

![](../../.gitbook/assets/1%20%2826%29.png)

```markup
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>부서관리-시작화면</title>
	<link rel="stylesheet" type="text/css" href="../../themes/default/easyui.css">
    <link rel="stylesheet" type="text/css" href="../../themes/icon.css">
    <link rel="stylesheet" type="text/css" href="../../demo/demo.css">
    <script type="text/javascript" src="../../js/jquery.min.js"></script>
    <script type="text/javascript" src="../../js/jquery.easyui.min.js"></script>
</head>
<body>

<table class="easyui-datagrid" style="width:400px;height:250px"
        data-options="url:'http://Localhost:9000/EasyUI/datagrid/dept.json',fitColumns:true,singleSelect:true">
    <thead>
        <tr>
            <th data-options="field:'DEPTNO',width:100,align:'center'">부서번호</th>
            <th data-options="field:'DNAME',width:100,align:'center'">부서명</th>
            <th data-options="field:'LOC',width:100,align:'center'">지역</th>
        </tr>
    </thead>
</table>

</body>

```

### dept.json

![WEB](../../.gitbook/assets/2%20%2819%29.png)

```markup
[
	{
		"LOC": "NEW YORK",
		"DEPTNO": 10,
		"DNAME": "ACCOUNTING"
	},
	{
		"LOC": "DALLAS",
		"DEPTNO": 20,
		"DNAME": "RESEARCH"
	},
	{
		"LOC": "CHICAGO",
		"DEPTNO": 30,
		"DNAME": "SALES"
	},
	{
		"LOC": "포천시",
		"DEPTNO": 41,
		"DNAME": "개발2팀"
	},
	{
		"LOC": "서울",
		"DEPTNO": 99,
		"DNAME": "개발부"
	},
	{
		"LOC": "대전",
		"DEPTNO": 50,
		"DNAME": "개발부"
	},
	{
		"LOC": "광주",
		"DEPTNO": 60,
		"DNAME": "개발부"
	},
	{
		"LOC": "울산",
		"DEPTNO": 70,
		"DNAME": "개발부"
	}
]
```

