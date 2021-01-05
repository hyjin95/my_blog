# Spring : 게시글 삭제

### 1. read.jsp : 상세보기페이지

```markup
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ page import = "java.util.List, java.util.Map, java.util.HashMap" %>
<%
	int tot = 0;
	List<Map<String,Object>> boardList = null;
	boardList = (List<Map<String,Object>>)request.getAttribute("boardDetail");
	Map<String,Object> rMap = new HashMap<>();
	if(boardList != null){
		rMap = boardList.get(0);
	}
%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>상세보기페이지[WEB-INF]</title>
<%@ include file="/common/easyUI_common.jsp" %>
<script type="text/javascript">
	function repleForm(){
		let url="repleForm.sp?bm_no="+<%=rMap.get("BM_NO").toString()%>;
		cmm_window_popup(url,"700px","500px","repleForm");
	}
	function refresh() {
		//alert("refresh호출");
		location.href="/board/boardList.sp";
	}

	function deleteView() {
		$('#dlg_boardDel').dialog({
		    title: '글 삭제',
		    buttons: btn_boardDel,
		    width: 400,
		    height: 250,
		    closed: true,
		    cache: false, //true : 메모리(캐시)에 있는것은 DB경유 없이 있는것을 사용한다.
		    href: 'boardDelForm.jsp?bm_no=<%=rMap.get("BM_NO")%>&bm_pw=<%=rMap.get("BM_PW")%>',
		    modal: true
		});
		$('#dlg_boardDel').dialog('open');
	}
	function deleteBoard() {
		//DB에서 가져온 비밀번호 담기
		//JS변수 선언시 let사용하기 : ES5추천방식
		//멤버변수와 지변의 차이, 버그 예방에 효율적
		//스킈립트는 컴파일을 하지 않기때문에 타입이 런타임시에 결정된다. = 파이썬도
		//순서, 절차 지향적인 성격(C, c++이 여기에 해당, (자바는 구조, 객체지향))
		let db_pw = "<%=rMap.get("BM_PW").toString()%>";
		//사용자가 입력한 비밀번호 담기, 위아래가 같아야 삭제가 가능하다.
		let u_pw = $("#u_pw").textbox('getValue');
		alert(db_pw+", "+u_pw);
		if(db_pw == u_pw) {
			//자바스크립트에서는 0이 아니면 모두가 참이다. r이 0이 아니라면 참이.
			$.messager.confirm('Confirm','삭제하시겠습니까?',function(r){
			    if (r){
			        //alert('ok');
			        location.href='boardDelete.sp?bm_no=<%=rMap.get("BM_NO")%>&bs_file=<%=rMap.get("BS_FILE")%>';
			    }
			});
		}
		
	}
	function deleteClose(){
		$('#dlg_boardDel').dialog('open');
	}
</script>
</head>
<body>
<!-- ===================== [[ 글 상세보기 화면 ]] ====================== -->
<table id="pan_read" class="easyui-panel" title="글상세보기" data-options="footer:'#tb_read'"
 style="width:670px;height:380px;padding:10px;background:#fafafa;">
   <tr>
      <td>제목</td>
      <td><input class="easyui-textbox" value="<%=rMap.get("BM_TITLE") %>" id="bm_title" data-options="width:'450px'"></td>
   </tr>
   <tr>
      <td>작성자</td>
      <td><input class="easyui-textbox" value="<%=rMap.get("BM_WRITER") %>" id="bm_writer" data-options="width:'450px'"></td>
   </tr>
   <tr>
      <td>이메일</td>
      <td><input class="easyui-textbox" value="<%=rMap.get("BM_EMAIL") %>" id="bm_email" data-options="width:'450px'"></td>
   </tr>
   <tr>
      <td>내용</td>
      <td><input class="easyui-textbox" value="<%=rMap.get("BM_CONTENT") %>" id="bm_content" data-options="multiline:'true',width:'450px', height:'90px'"></td>
   </tr>
   <tr>
      <td>비밀번호</td>
      <td><input class="easyui-textbox" value="<%=rMap.get("BM_PW") %>" id="bm_pw" data-options="width:'450px'"></td>
   </tr>
</table>
 <div id="tb_read" style="padding:2px 5px;" align="center">
    <a href="javascript:repleForm()" class="easyui-linkbutton" iconCls="icon-edit" plain="true">댓글쓰기</a>
    <a href="javascript:updateForm()" class="easyui-linkbutton" iconCls="icon-add" plain="true">수정</a>
    <a href="javascript:deleteView()" class="easyui-linkbutton" iconCls="icon-remove" plain="true">삭제</a>
    <a href="javascript:boardList()" class="easyui-linkbutton" iconCls="icon-search" plain="true">목록</a>
</div>
<!-- ===================== [[ 글 삭제하기 화면 ]] ====================== -->
<div id="dlg_boardDel" closed="true" class="easyui-dialog" style="padding:20px 50px">
	<div id="btn_boardDel" alilgn="right">
		<a href="javascript:deleteBoard()" class="easyui-linkbutton" iconCls="icon-ok" plain="true">확인</a>
		<a href="javascript:deleteClose()" class="easyui-linkbutton" iconCls="icon-cancel" plain="true">닫기</a>
	</div>
</div>
</body>
</html>
```

