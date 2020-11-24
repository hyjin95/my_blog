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
		$('#tre_movie').tree({
			onClick: function(node){
				if(node.text=='회원목록'){
					if($.cookie("cname")!=null){
						alert("회원목록 페이지 열어주기");
					}else{
						alert("로그인 후 이용할 수 있습니다");
						return;
					}
				}
			}
		});
	});
</script>
```

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



