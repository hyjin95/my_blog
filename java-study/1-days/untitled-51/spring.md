# Spring : 첨부파일 업로드

## 1. 첨부파일 등록 : View

### 코드 : boardList.jsp \[WEB_INF]

```markup
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ page import = "java.util.List, java.util.Map, java.util.HashMap" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>게시판 목록[WEB-INF/views]</title>
</script>
</head>
<body>
<script type="text/javascript">
	function addForm(){//var가 아닌 ECMAScript에서 제안하는 let예약어를 사용하자. -React.js, Vue.js, TypeScript.js를 위해서는 let이 더 안전하다.
		let url="writeForm.sp";
		cmm_window_popup(url,"700px","500px","writeForm");
	}
</script>

 <div id="tb_board" style="padding:2px 5px;">
    <a href="javascript:addForm()" class="easyui-linkbutton" iconCls="icon-edit" plain="true">입력</a>
</div> 
</body>
</html>
```

* 입력 버튼을 누르면 addForm( )함수가 실행되어 writeForm화면이 popup형태로 띄워진다.

### 코드 : boardController.java

```java
public void writeForm(HttpServletRequest req, HttpServletResponse res)
		throws Exception{
		logger.info("controller - writeFrom호출성공");
		res.sendRedirect("writeForm.jsp");
	}
```

### 코드 : writeForm.jsp \[webapp]

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
     // $("#f_write").attr("method","post");
      //$("#f_write").attr("action","/board/boardInsert.sp");
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
	<form id="f_write" action="/board/boardInsert.sp" method="post" enctype="multipart/form-data">
	<!-- numberFormatException방지, 사용자가 입력하는 값이 아님 -->
	<input type="hidden" name="bm_no" value="0"/>
	<input type="hidden" name="bm_group" value="0"/>
	<input type="hidden" name="bm_pos" value="0"/>
	<input type="hidden" name="bm_step" value="0"/>
	<table>
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
</div>
</html>
```

* 파일을 등록하고 저장 버튼을 클릭하면 addAction함수가 실행되고 f_write from이 전송된다.

## 2. 첨부파일 등록 처리 : Controller

### 코드 : boardController.java

```java
package com.board;

import java.util.HashMap;
import java.util.List;
import java.util.Map;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.springframework.web.multipart.MultipartFile;
import org.springframework.web.servlet.mvc.multiaction.MultiActionController;

import com.util.HashMapBinder;

public class BoardController extends MultiActionController {//spring제공 클래스
	
	private BoardLogic boardLogic = null;

	public void setBoardLogic(BoardLogic boardLogic) {
		this.boardLogic = boardLogic;
	}
	
	//첨부파일은 필수조건이 아니므로 nullPointer를 피하려면 required를 붙여줘야 한다.
	public void boardInsert(HttpServletRequest req, HttpServletResponse res)
			throws Exception{
		int result = 0;
		Map<String,Object> pmap = new HashMap<>();
		HashMapBinder hmb = new HashMapBinder(req);
		//첨부파일을 처리할 때에는 반드시 post방식이여야 한다.
		//주의사항 : request로는 사용자가 입력한 값을 받을 수가 없다.
		//따라서 cos.jar에서 제공하는 MultipartRequest를 사용해야만 값을 요청할 수 있다.
		hmb.multiBind(pmap);
		result = boardLogic.boardInsert(pmap);//첨부파일 여부는 logic에서 처리한다.
		if(result==1) {
			res.sendRedirect("/board/boardInsertOk.jsp");//insert성공 후 목록 갱신, 보여주기
		}else {
			res.sendRedirect("/board/boardInsertFail.sp");
		}
	}
```

* 첨부파일은 반드시 post방식으로 전송되어야 처리가 가능하다.
* 전송된 입력양식을 HashMapBind클래스를 활용해 파라미터로 넘길 pmap에 담는다.
* insert성공시 webapp > boardInsertOk.jsp를 호출한다.

### 코드 : HashMapBinder.java

```java
package com.util;

import java.io.File;
import java.util.Enumeration;
import java.util.Map;

import javax.servlet.http.HttpServletRequest;

import org.apache.log4j.Logger;

import com.oreilly.servlet.MultipartRequest;
import com.oreilly.servlet.multipart.DefaultFileRenamePolicy;

public class HashMapBinder {
	Logger logger = Logger.getLogger(HashMapBinder.class);
	
	//post방식으로 첨부파일을 처리할때 request대신 사용할 객체
	//이것이 있어야 사용자가 입력한 값을 받을 수 있다.
	public MultipartRequest multi = null;
	//첨부파일 한글처리
	String encType = "utf-8";
	//첨부파일의 크기
	int maxSize = 10*1024*1024;//10MB까지
	//첨부파일의 물리적 위치
	String realFolder = "";
	
	public HttpServletRequest request = null;
	
	public HashMapBinder() {	}
	
	public HashMapBinder(HttpServletRequest request) {
		this.request = request;//원본을 사용해야한다.	서블릿에서 인스턴스화	
		//오라클에 관리하지 않고 사진 같이 용량이 있는 자료는 따로 이렇게 폴더를 만들어 관리하는 것이 보편적이다. 
		//톰캣이 바라보는 물리적 경로, 여기에 업로드된다. - web에서 접근이 가능해진다.
		realFolder = "C:\\workspace_sts3\\spring3_1\\src\\main\\webapp\\pds";
	}
```

* 기존의 request객체 대신 cos.jar가 제공하는 MultipartRequest객체를 사용한다.
* 19번에서 멤버변수로 선언
* 21번은 첨부파일에 대한 인코딩타입을 정하는 코드이다.
* 23번에서는 멤버변수로 최대 사이즈로 사용할 사이즈를 정한다.
* 25번에서는 첨부파일의 물리적위치를 담을 String변수를 선언한다.
* 31번 생성자에서 HashMapBinder객체가 생성될때 파일의 물리적 경로를 지정하게 된다.\
  사진과 같은 파일 자료는 오라클에 관리하기 버겁기 때문에 따로 폴더에 관리하는 것이 일반적이다.\
  위와 같이 경로를 지정하면 톰캣이 web에서도 접근할 수 있게되는 것이다.

```java
	//첨부파일이 있는 경우
	public void multiBind(Map<String,Object> target) {
		target.clear();
		//첨부파일의 경우 예외처리 필수
		try {
			//요청객체, 물리적위치, 최대크기, 인코딩타입, 파일정책관리인자
			multi = new MultipartRequest(request, realFolder, maxSize, encType, new DefaultFileRenamePolicy());
		} catch (Exception e) {
			logger.info("Exception : "+e.toString());
		}
		logger.info("multi : "+multi);
		Enumeration<String> en = multi.getParameterNames();
		while(en.hasMoreElements()) {
			String key = en.nextElement();
			target.put(key, multi.getParameter(key));
		}
```

* 3번에서 값을 담을 파라미터 target을 clear한다.
* 5번 : 첨부파일 처리에는 예외처리가 필수이다.
* 7번 : 예외처리 안에서 MultipartRequest객체를 생성한다.\
  \- 파라미터 : 요청객체, 물리적위치, 최대크기, 인코딩타입, 파일정책관리인자
* 12번 : 자료구조에 접근할 수 있는 인터페이스 Enumeration을 사용해 요청에 접근한다.
* 13번 : 자료구조에 값이 있는 동안에는
* 14번 : String 변수에 key를 담는다.
* 15번 : 파라미터로 받은 map에 "key", 값 을 담는다.
* 여기까지는 파일이 아닌 다른 입력값들을 담아주는 과정이다.

```java
		//첨부파일에 대한 정보를 받아오는 코드 추가
		Enumeration<String> files = multi.getFileNames();
		if(files != null) {
			//읽어온 파일 이름을 객체로 만들어 준다. 파일이름을 클래스로 객체화시켜준다. 자바코드 안에서 사용하려면 객체여야 한다.
			File file = null;
			while(files.hasMoreElements()) {
				String fname = files.nextElement();
				String filename = multi.getFilesystemName(fname);
				logger.info("filename : "+filename);
				//사용자가 선택한 파일명 담기 - null이 아니라면 첨부파일이 존재하는 것이다. --1
				target.put("bs_file",  filename);
```

* 이제 파일 이름을 담는 과정이다. - bs_file
* 똑같이 Enumeration인터페이스를 활용해 MultipartRequest가 제공해주는 getFileName( )메서드에 접근해 파일 이름의 존재 여부를 확인한다.
* 3번 : 파일이름이 있다면 = 첨부파일이 존재한다면
* 5번 : File타입 변수를 선언한다.
* 6번 : key값이 있는 동안에는
* 7번 : String변수에 key값을 담는다.
* 8번 : 파일이름을 getFilesystemName(key)메서드를 활용해 꺼내 String변수에 담는다.
* 11번 : 파라미터에 담는다.

```java
				//파일 크기를 계산하려면 파일 명으로 파일 객체를 생성해야 length함수를 사용할 수 있다.
			 if(filename != null && filename.length()>1) {
					file = new File(realFolder + "\\" + filename);
					logger.info("file : "+file);
				}
			}///////////end of while
			//첨부파일의 크기 담기 --2
			double size = 0;//int보다 큰 double을 사용한다.
			if(file != null) {
				size = file.length();
				size = size/1024;//kb
				target.put("bs_size", size);
			}
		}
	}
```

* 이제 파일 사이즈를 담는 과정이다. - bs_size
* 파일의 크기를 계산하려면 File객체로 length함수를 사용해야 한다.\
  이를 위해 파일 이름으로 File객체를 생성한다.
* 2번 : 가져온 파일이름이 존재하고 문자열이 0이상이라면
* 3번 : 물리적경로+파일이름 으로 File의 실제를 담은 File객체를 생성한다.
* 8번 : size를 담을 double변수 선언
* 9번 : file이 있다면
* 10번 : double변수에 file.length( )를 담는다.
* 11번 : 수식을 활용해 kb단위로 계산한다.
* 12번 : 파라미터에 담는다.
* 여기까지 하면 pmap(=target)에는 bm_no, bsfile, bs_size가 담기게된다.

## 3. 첨부파일 등록 처리 : Model

### 코드 : boardLogic.java

```java
public int boardInsert(Map<String, Object> pMap) {//트랜잭션처리
		logger.info("Logic - boardInsert 호출 성공 : "+pMap);
		int result = 0;
		int bm_no = 0;
		int bm_group = 0;
		int bs_seq = 0;
		if(pMap.get("bm_group")!=null) {
			bm_group = Integer.parseInt(pMap.get("bm_group").toString());
		}
		bm_no = sqlBoardMDao.getBmno();//pk이므로 마스터에서 가져온다.
		pMap.put("bm_no",bm_no);
		//새글이니?
		if(bm_group > 0) {//pos, step이 0이다.
			bm_group = sqlBoardMDao.getBmGroup();//그룹번호는 채번하는 거라 파라미터는 필요없다.
			pMap.put("bm_pos", 0);
			pMap.put("bm_step", 0);
		}
		int mresult = sqlBoardMDao.boardMInsert(pMap);
		//첨부파일이 있니?
		if(pMap.get("bs_file") != null && pMap.get("bs_file").toString().length() > 1) {
			pMap.put("bm_no", bm_no);
			bs_seq = Integer.parseInt(pMap.get("bs_seq").toString());
			pMap.put("bs_seq", bs_seq);
			int dresult = sqlBoardDDao.boardDInsert(pMap);
		}
		return mresult;
	}
```

### SQL

```markup
	<insert id="boardMInsert" parameterType="map"><!-- 끝에 세미콜론 들어가면 안됨!!! -->
		INSERT INTO board_master_t(bm_no,bm_writer,bm_title,bm_content,bm_email,bm_date,bm_group,bm_pos,bm_step,bm_pw)
       	 VALUES(#{bm_no},#{bm_writer},#{bm_title},#{bm_content},#{bm_email},TO_CHAR(SYSDATE,'YYYY-MM-DD'),#{bm_group},#{bm_pos},#{bm_step},#{bm_pw})
	</insert>	  
	
	<insert id="boardDInsert" parameterType="map"><!-- 끝에 세미콜론 들어가면 안됨!!! -->	   
		INSERT INTO board_sub_t(bm_no, bs_seq, bs_file, bs_size)
		   VALUES(#{bm_no},#{bs_seq},#{bs_file},#{bs_size})
	</insert>
```

## 4. 처리결과 반영

### 코드 : boardInsertOk.jsp \[webapp]

```markup
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<script>
   alert("등록되었습니다.")
   opener.location.href="javascript:refresh()";
   self.close();
   //location.href="/board/boardList.sp";
</script>
```

* writeForm popup화면을 닫고 목록 화면을 새로고침한다.

### 코드 : boardList.jsp

```javascript
	function refresh() {
		//alert("refresh호출");
		location.href="/board/boardList.sp";
	}
```

```markup
<body>
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
    <!---================== 조회결과가 있는 경우 ======================= -->    	
    <%
    	if(tot > 0){
    		for(int i=0;i<tot;i++){
    			Map<String,Object> rmap = boardList.get(i);    		   	
    %>
    	<tr>
    		<td><%=rmap.get("BM_TITLE") %></td>
         <td><%=rmap.get("BM_WRITER") %></td>
         <td><%=rmap.get("BM_DATE") %></td>
         <td>
            <%
            	if(!"해당없음".equals(rmap.get("BS_FILE").toString())){
            %>
            	<a href="javascript:download('<%=rmap.get("BS_FILE") %>')">
            	<%=rmap.get("BS_FILE") %>
            	</a>
            <%
            	}
            	else{
            %>
            	<%="해당없음" %>
            <%
            	}
            %>
            </td>
            <td><%=rmap.get("BM_HIT") %></td>
       </tr>
     <%
    		}
    	}
    		
     %>
    </tbody>
 </table>  
</body>
</html>
```

* 스크립틀릿과 if문을 활용해 첨부파일이 없다면 "해당없음"으로 표시하고, 첨부파일이 존재한다면 다운로드함수를 호출하도록 한다.

### 결과

![](<../../../.gitbook/assets/1 (104).png>)
