# cookie Test

### 화면1 : 기본

![](<../../../.gitbook/assets/6 (19).png>)

### 화면2 : 생성 -> 값&#x20;

![](<../../../.gitbook/assets/7 (13).png>)

* 쿠키생성 버튼을 누르고 쿠키값 확인버튼을 누르면 생성된 쿠키가 찍히는 것을 볼 수 있다.
* 이 상황에서 페이지 이동으로 다른 페이지를 갔다 뒤로가기 해서 오거나, 새로고침해도 cookie는 유지된다.

### 화면3 : 수정 -> 값

![](<../../../.gitbook/assets/8 (8).png>)

* 수정을 하고 쿠키값 확인 버튼을 누르면 c\_id가 바뀐것을 볼 수 있다.

### 화면4 : cookie 삭제

![](<../../../.gitbook/assets/5 (26).png>)

* clear하면 해당 페이지에서 생성된 쿠키가 삭제된다.
* 쿠키 삭제 버튼을 누르면 c\_id만 null이 된다.

### 코드 : cookieTest.jsp

```markup
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>쿠키관리[jquery API활용하기]</title>
<style type="text/css">
	#d_cookie{
		border: 1px solid black;
		background: pink;
		width: 500px;
		height: 250px;
	}
</style>
<script type="text/javascript" src="https://code.jquery.com/jquery-3.4.1.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery-cookie/1.4.1/jquery.cookie.min.js"></script>
</head>
<body>
<script type="text/javascript">
	$(document).ready(function(){
		$("#btn_make").click(function(){
			$.cookie('c_id','tomato');//쿠키를 상수값으로 만들기, path를 주지않으면 내가 바라보는 곳을 가르킨다.현재 위치, 여기서만 유지되어 다른 폴더의 브라우저에서는 유지되지않는다.
			//한글도 될까?, path인 동안 유지한다., secure는 true를 주면 안된다.
			$.cookie('c_gender','남자',{expires:1, path:'/dev_html/ajax', domain:'localhost', secure: false});//path가 달라 보이지않는다.
			$.cookie('c_name','이순신',{expires:7, path:'/', domain:'localhost', secure: false});
		});
		$("#btn_update").click(function(){
			$.cookie('c_id','apple');
		});
		$("#btn_delete").click(function(){
			$.cookie('c_id',null);
		});
		$("#btn_move").click(function(){//페이지를 이동해도 유지될까?
			location.href="cookieMove.jsp"
		});
		$("#btn_confirm").click(function(){//쿠키값 찍기
			$("#d_cookie").append("<br>"+'c_id : '+$.cookie('c_id')+"<br>")
			$("#d_cookie").append('c_gender : '+$.cookie('c_gender')+"<br>")
			$("#d_cookie").append('c_name : '+$.cookie('c_name')+"<br>")
		});
	});
</script>
<div id="d_cookie">쿠키정보 전광판</div>
<br><br>
<button id="btn_make">쿠키생성</button>
<button id="btn_update">쿠키수정</button>
<button id="btn_delete">쿠키삭제</button>
<button id="btn_move">페이지이동</button>
<button id="btn_confirm">쿠키값 확인</button>
</body>
</html>
```

### 설명

* 코드의 23, 25, 26번의 path에 주의해야한다.
* path를 벗어나는 페이지에서는 cookie가 유지되지않는다.
* 25번의 c_gender라는 cookie는_ dev\_html프로젝트의 ajax경로를 갖는 페이지 에서만 유지된다.
* 26번은 같은 루트태그인 페이지에서 유지된다.
* 23번과 같이 path가 지정되지 않으면 현재 쿠키가 위치한 페이지의 현재 위치를 path로 한다.\
  \- 같은 문서내의 페이지라면 유지된다.
