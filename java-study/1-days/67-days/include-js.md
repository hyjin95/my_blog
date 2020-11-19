# include : 디렉티브로 JS 가져오기 구현

## include : 디렉티브로 JS 가져오기

### 화면 

![](../../../.gitbook/assets/3%20%2842%29.png)

* jquery와 easyui에 대한 import페이지를 정상적으로 불러왔음을 확인 할 수 있다.
* 화면 우클릭 - 페이지 소스보기를 하면 해당 코드들이 모두 들어와 있음을 확인 할 수 있다.

### 문서 직접 확인하기 : 경로

![&#xACBD;&#xB85C;](../../../.gitbook/assets/1%20%2869%29.png)

![step1.jsp &#xBA54;&#xBAA8;&#xC7A5;](../../../.gitbook/assets/2%20%2854%29.png)

* 외부에 작성된 코드가 들어와있는 것을 확인할 수 있다.

### 코드 : step1.jsp

```markup
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<!-- 공통코드 시작 -->
<!-- 공통코드로 작성하는 이유
	 같은 프로젝트, 같은 UI/UX를 사용한다면 중복되는 코드는 한번만 작성하는게 효율적이다. 
	 jsp상에서 선언문<link>도 중복되므로 공통코드로 만들자. -->
<%@ include file="/common/easyUI_common.jsp" %>
<!-- 공통코드 종료 -->
</head>
<body>
<div id="d_result">여기</div>
<script type="text/javascript">
</script>
</body>
</html>
```

### 코드 : easyUI\_common.jsp - JS&lt;Link&gt;

```markup
<!-- 소스가 하나로 합쳐지니까 선언문이 두 번 올 필요가 없다. -->
<link rel="stylesheet" type="text/css" href="https://www.jeasyui.com/easyui/themes/default/easyui.css">
<link rel="stylesheet" type="text/css" href="https://www.jeasyui.com/easyui/themes/icon.css">
<link rel="stylesheet" type="text/css" href="https://www.jeasyui.com/easyui/themes/color.css">
<link rel="stylesheet" type="text/css" href="https://www.jeasyui.com/easyui/demo/demo.css">
<script type="text/javascript" src="https://www.jeasyui.com/easyui/jquery.min.js"></script>
<script type="text/javascript" src="https://www.jeasyui.com/easyui/jquery.easyui.min.js"></script>
```

