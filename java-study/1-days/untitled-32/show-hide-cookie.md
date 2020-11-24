# 로그아웃 : show, hide, cookie

## 코드 : indexVer2.jsp 

### JS : function loginAction\( \)

```javascript
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>로그인 처리 후[indexVer2.jsp]</title>
<!-- 공통코드 include처리 -->
<%@ include file="/common/easyUI_common.jsp" %>

<script type="text/javascript">
	function loginAction(){
		var u_id = $("#mem_id").val();
		var u_pw = $("#mem_pw").val();
		//비번이 있으므로 post방식 사용하자.
		var param = "mem_id="+u_id+"&mem_pw="+u_pw;
		$.ajax({
			 url:'loginAction3.jsp'
			,type:'post'
			,data: param
			,dataType:'text'
			,success:function(data){
				alert(data);
				 if("아이디가 존재하지 않습니다."==data){
	                  return;//함수탈출for문의 break, 처음부터 다시 사용할 것
	               }
	               else if("비밀번호가 틀렸습니다."==data){
	                  return;
	               }else{
	                  $.cookie("cname",data);
	                  $("#d_login").hide();
	                  $("#d_logout").show();
	               }				
			}
		});////end of ajax
	}//////end of loginAction
```

* 로그인 버튼을 클릭하면 호출되는 함수
* 20번의 data속성을 post방식을 사용하기위한 ajax 속성이다.
* 22-26번 : 로그인 인증에서 아이디가 존재하지 않는다면 if문을 종료한다.
* 27-28번 : 비밀번호가 맞지않는다면 if문을 종료한다.
* 29-33번 : 로그인 인증에 성공한다면 cookie에 사용자 이름을 담고, 로그인화면을 hide, 로그인 성공 화면을 show한다.
* 이때 저장된 cookie는 ajax 비동기통신이 완료되고 브라우저가 다운로드해야 브라우저에서 인식한다.

### JS : function logoutAction\( \)

```javascript
	function logoutAction(){
		$("#mem_id").textbox('setValue','');
		$("#mem_pw").passwordbox('setValue','');
		$("#d_login").show();
		$("#d_logout").hide();
		$.cookie('cname',null,{expires:-1});
	}
</script>
</head>
```

* 로그인 성공 후 로그아웃 버튼을 클릭하면 호출되는 함수
* 2-4번 : 기존 로그인 화면의 id, pw입력창을 초기화하고 show한다. 
* 5번 : 로그인 성공시 보여줬던 화면은 hide한다.
* 6번 : 쿠키를 초기화한다.

### &lt;body&gt; : $\(document\).ready

```markup
<body>
<script type="text/javascript">
	$(document).ready(function(){
		var cname = null;//cookie에서 가져온 이름 담기
		cname = $.cookie("cname");
		//로그인 한거니? --if문
		if(cname==null){//cname이 null이면 로그인 인증되지 않은 사용자
			$("#d_login").show();
			$("#d_logout").hide();
		}else {//로그인 인증완료한 사용자
			$("#d_logout").show();
			$("#d_login").hide();
		}
	});
</script>
```

* 처음 브라우저에 화면이 로드될때 로그인 여부를 확인한다.
* 7-9번 : cookie가 null이라면 로그인 정보가 없는 것이므로 로그인 화면을 show한다.
* 10-12번 : cookie에 사용자이름이 있다면 로그인한 사용자가 있다는 것이므로 로그인 성공 화면을 show한다.

### &lt;body&gt; : 로그인 화면

```markup
<!-- EasyUI Layout -->
<div id="lay-movie" class="easyui-layout" style="width:1000px;height:500px;">
	<div data-options="region:'west',title:'영화나무',split:true" style="width:200px;">
		<!-- 마진속성을 이용해 간격 벌리기 -->
		<div style="margin:10px 0;"></div>
	
		<!---------------- [[ 로그인 영역 ]] -------------------->
		<div id="d_login" align="center">
			<div style="margin:3px 0;"></div>
			<input id="mem_id" name="mem_id" class="easyui-textbox">
			<script type="text/javascript">
				$("#mem_id").textbox({
					 iconCls:'icon-man'
					,iconAlign: 'right'
					,prompt:'아이디'
				});
			</script>
			<div style="margin:3px 0;"></div>
				<input id="mem_pw" name="mem_pw" class="easyui-passwordbox">
				<script type="text/javascript">
					$("#mem_pw").passwordbox({
						 iconAlign:'right'
						,prompt:'패스워드'
						,showEye:true
					});
				</script>
			<div style="margin:3px 0;"></div>
				<a href="javascript:loginAction()" class="easyui-linkbutton" data-options="iconCls:'icon-man'">로그인</a>			
				<a href="javascript:memberAction()" class="easyui-linkbutton" data-options="iconCls:'icon-add'">회원가입</a>			
		</div>
		<!---------------- [[ 로그인 영역 ]] -------------------->
```

### &lt;body&gt; : 로그인 성공시 화면

```markup
		<!---------------- [[ 로그아웃 영역 ]] -------------------->
		<div id="d_logout" align="center">
			<div style="margin:5px 0;"></div>
			<div id="d_ok"><%="이순신"%>님 환영합니다.</div>
			<div style="margin:3px 0;"></div>
				<a href="javascript:logoutAction()" class="easyui-linkbutton" data-options="iconCls:'icon-man'">로그아웃</a>			
				<a href="javascript:memberUpdate()" class="easyui-linkbutton" data-options="iconCls:'icon-lock'">정보수정</a>			
		</div>	
		<!---------------- [[ 로그아웃 영역 ]] -------------------->
	</div>
</div>
</body>
</html>
```

* 4번은 변수로 처리해 사용자마다 다른 화면을 제공해야 한다.

## 코드 : loginAction3.jsp

```java
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
	String db_id = "test";
	String db_pw = "123";
	String db_name = null;
	String u_id = request.getParameter("mem_id");
	String u_pw = request.getParameter("mem_pw");

	if(u_id.equals(db_id)){
		if(u_pw.equals(db_pw)){
			db_name = "이순신";
		}else{
			db_name = "비밀번호가 틀렸습니다.";
		}
	}else{
		db_name = "아이디가 존재하지 않습니다.";
	}
%>
<%=db_name%>
```

* 로그인 인증을 처리하는 jsp
* 원래는 서블릿 - Logic - Dao - DB를 거쳐 인증과정을 거친다.
* 임시로 인증을 테스트하기위한 페이지
* 요청 파라미터 id가 db\_id와 일치하지않으면 "아이디가 존재하지 않습니다."
* 요청 파리미터 id는 일치하지만 pw가 일치하지 않으면 "비밀번호가 틀렸습니다."
* id와 pw가 모두 일피하면 "이순신" 을 출력한다.

