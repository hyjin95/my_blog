# Spring : 새 게시글 등록

## JAVA코드

{% page-ref page="../untitled-47/spring.md" %}

## JSP : WEB-INF/views/board

### 코드 : boardList.jsp

```markup
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ page import = "java.util.List, java.util.Map, java.util.HashMap" %>
<%
	int tot = 0;
	List<Map<String,Object>> boardList = null;
	boardList = (List<Map<String,Object>>)request.getAttribute("boardList");
	if(boardList != null){
		tot = boardList.size();
	}
%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>게시판 목록[WEB-INF/views]</title>
<%@ include file="/common/easyUI_common.jsp" %>
<script type="text/javascript">

	//상세보기 페이지로 이동 : read.jsp, 오라클을 경유하며 select이지만 조건절이 포함된다.
	//로직과 dao는 동일하게 사용하더라도 컨트롤 계층의 메서드는 다르게 정의한다. -응답페이지가 다르므로
	//select : boardList.jsp or read.jsp
	function boardDetail(p_bmno){
		location.href="/board/boardDetail.sp?bm_no="+p_bmno
	}
	function refresh() {
		//alert("refresh호출");
		location.href="/board/boardList.sp";
	}
</script>
</head>
<body>
<script type="text/javascript">
	function addForm(){
		let url="writeForm.sp";
		cmm_window_popup(url,"700px","500px","writeForm");
	}
</script>
게시판 목록
<!-- ======================== [[글목록 화면 시작]] =========================
JEasyUI의 DataGrid API를 활용하여 작성
1)익스프레션을 이용해서 화면 처리
  :tr, td태그를 직접 작성해서 처리하는 방식
2)json포맷으로 처리해서 매핑
  :field명만 맞춰주면 자동으로 매핑
 -->
 <table id="dg_board" title="글목록" style="width:950px;height:500px" class="easyui-datagrid" data-options="singleSelect:'true',toolbar:'#tb_search,#tb_board',footer:'#pn_board'">
    <!--헤더부분 추가 -->
    <thead>
       <tr>
          <th data-options="field:'BM_TITLE',width:'350px'">제목</th>
            <th data-options="field:'BM_WRITER',width:'100px'">작성자</th>
            <th data-options="field:'BM_DATE',width:'110px'">작성일</th>
            <th data-options="field:'BS_FILE',width:'280px'">첨부파일</th>
            <th data-options="field:'BM_HIT',width:'100px'">조회수</th>
       </tr>
    </thead>
    <!--데이터 출력 영역  -->
    <tbody>
    <!---================== 조회결과가 없는 경우 ======================= -->
    <%
    	if(tot==0){
    %>
    	<tr>
    		<td colspan="5">&nbsp;조회 결과가 없습니다.</td>
    	</tr>
    <%	
    	}
    %>
    	
    <!---================== 조회결과가 있는 경우 ======================= -->    	
    <%
    	if(tot > 0){
    		for(int i=0;i<tot;i++){
    			Map<String,Object> rmap = boardList.get(i);    		   	
    %>
    	<tr>
    		<td>
	    		<a href="javascript:boardDetail('<%=rmap.get("BM_NO")%>')">
            	<%=rmap.get("BM_TITLE") %>
            	</a>
            </td>
            <td><%=rmap.get("BM_WRITER") %></td>
            <td><%=rmap.get("BM_DATE") %></td>
            <td><%="첨부파일" %></td>
            <td><%=rmap.get("BM_HIT") %></td>
       </tr>
     <%
    		}
    	}    		
     %>
    </tbody>
 </table>  
 
 <!--======================= [[페이지 네이션 추가]] ======================-->
 <table id="dg_board" title="글목록" style="width:950px;height:20px">
    <tr>
    <td align="center" >
       <font size="5">1 2 3 4 5 6 7 8 9 10</font>
    <br>
 
  <!-- 조건검색 추가(툴바하기) -->
 <div id="tb_search" style="padding:2px 5px;">
    <select id="cb_search" name="cb_search" class="easyui-combobox" panelHeight="auto" style="width:100px">
       <option selected value="">선택</option>
       <option value="bm_title">제목</option>
       <option value="bm_content">내용</option>
       <option value="bm_writer">작성자</option>
    </select>
    <input class="easyui-textbox" id="keyword" name="keyword" style="width:320px">
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
    작성일:<input class="easyui-datebox" id="bm_date" name="bm_date" style="width:120px">
 </div>
 <div id="tb_board" style="padding:2px 5px;">
    <a href="javascript:boardList()" class="easyui-linkbutton" iconCls="icon-search" plain="true">조회</a>
    <a href="javascript:addForm()" class="easyui-linkbutton" iconCls="icon-edit" plain="true">입력</a>
    <a href="javascript:updForm()" class="easyui-linkbutton" iconCls="icon-add" plain="true">수정</a>
    <a href="javascript:delForm()" class="easyui-linkbutton" iconCls="icon-remove" plain="true">삭제</a>
</div> 
</body>
</html>
```

## JSP : webapp/board

### 코드 : writeForm.jsp

```markup
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>글쓰기 화면[webapp]</title>
<%@ include file="/common/easyUI_common.jsp" %>
<script>
   function addAction(){
      $("#f_write").attr("method","post");
      $("#f_write").attr("action","/board/boardInsert.sp");
      $("#f_write").submit();
   }
   function writeClose(){
      window.close();
   }
</script>

</head>
<body>
</body>
<div id="dlg_boardAdd" class="easyui-panel" title="글쓰기" style="width:650px;height:450px;padding:10px" data-options="footer:'#tbar_boardAdd'">
	<form id="f_write">
	<!-- numberFormatException방지, 사용자가 입력하는 값이 아님 -->
	<input type="hidden" name="bm_no" value="0"/>
	<input type="hidden" name="bm_group" value="0"/>
	<input type="hidden" name="bm_pos" value="0"/>
	<input type="hidden" name="bm_step" value="0"/>
	<table>
      <tr>
         <td width="100px">제목</td>
         <td width="500px">
            <input class="easyui-textbox" data-options="width:'350px'" id="bm_title" name="bm_title" required>
         </td>
      </tr>
      <tr>   
         <td width="100px">작성자</td>
         <td width="500px">
            <input class="easyui-textbox" data-options="width:'150px'" id="bm_writer" name="bm_writer" required>
         </td>
      </tr>
      <tr>
         <td width="100px">이메일</td>
         <td width="500px">
            <input class="easyui-textbox" data-options="width:'250px'" id="bm_email" name="bm_email">
         </td>
      </tr>
      <tr>         
         <td width="100px">내용</td>
         <td width="500px">
            <input class="easyui-textbox" id="bm_content" name="bm_content" data-options="multiline:'true',width:'400px',height:'90px'" required>
         </td>
      </tr>
      <tr>         
         <td width="100px">비번</td>
         <td width="500px">
            <input class="easyui-textbox" data-options="width:'100px'" id="bm_pw" name="bm_pw" required>
         </td>
      </tr>
      <tr>         
         <td width="100px">첨부파일</td>
         <td width="500px">
            <input class="easyui-filebox" data-options="width:'450px'" id="bs_file" name="bs_file">
         </td>
      </tr>
   </table>
   </form>
</div>
<!-- 입력 화면 버튼 추가 -->
<div id="tbar_boardAdd" align="right">
   <a href="javascript:addAction()" class="easyui-linkbutton" iconCls="icon-save">저장</a>
   <a href="javascript:writeClose()" class="easyui-linkbutton" iconCls="icon-cancel">닫기</a>
</div>
</html>
```

### 코드 : boardInsertOk.jsp

```javascript
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<script>
   alert("등록되었습니다.")
   opener.location.href="javascript:refresh()";
   self.close();
   //location.href="/board/boardList.sp";
</script>
```

## JPanel로 팝업창 만들기

### webaap &gt; js &gt; commons.js

```javascript
/*********************************************************
 * 윈도우 팝업창 처리 구현
 * @param1:화면에 띄울 페이지의 URL
 * @param2:팝업창의 가로
 * @param3:팝업창의 세로
 * @param4:팝업창 이름
 *********************************************************/
function cmm_window_popup(url,popupwidth,popupheight,popupname)
{
	//해상도(1600*1024) , 팝업창크기(700,450)
	Top = (window.screen.height-popupheight)/3;//(1024-450)/191
	Left = (window.screen.width-popupwidth)/2;//(1600-700)/450
	if(Top<0) Top = 0;
	if(Left<0) Left = 0;
	options = "location=no, fullscreen=no, status=no";
	options+=", left="+Left+", top="+Top;
	options+=", width="+popupwidth+", height="+popupheight;
	popupname = window.open(url,popupname,options);
	//popupname.focus();
}
```

### web &gt; common &gt; easyUI\_common.jsp

```markup
<script type="text/javascript" src="../js/commons.js"></script>
```

