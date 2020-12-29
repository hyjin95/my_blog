# Spring : 첨부파일 다운로드

## 코드

### 코드 : boardList.jsp\[WEB-INF\]

```javascript
	function download(pbs_file){
		location.href="./downLoad.jsp?bs_file="+pbs_file;
	}
```

* 목록의 첨부파일을 클릭하면 호출되는 함수
* downLoad.jsp를 호출한다. 파라미터로받은 pbs\_file을 같이 넘겨준다.

```markup
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

### 코드 : downLoad.jsp

```java
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ page import="java.io.*,java.net.*" %>    
<%
	out.clear(); //out--> jsp자체 객체
	out=pageContext.pushBody();

   String b_file = request.getParameter("bs_file");
   String fname = b_file;
   out.print("b_file: 8->euc"+b_file);      
   out.print("<br>");      
   String filePath = "C:\\workspace_sts3\\spring3_1\\src\\main\\webapp\\pds\\"; // 절대경로.   
   File file = new File(filePath,b_file.trim());
    String mimeType = getServletContext().getMimeType(file.toString());
   if(mimeType == null){//마임타입
      response.setContentType("application/octet-stream");
   }
   String downName = null;
   if(request.getHeader("user-agent").indexOf("MSIE")==-1){
      downName = new String(b_file.getBytes("UTF-8"),"8859_1");
   }else{
      downName = new String(b_file.getBytes("EUC-KR"),"8859_1");
   }
      response.setHeader("Content-Disposition", "attachment;filename="+downName);
    FileInputStream fis = new FileInputStream(file);
   ServletOutputStream sos = response.getOutputStream();
   try{
      byte b[] = new byte[1024*10];
      int data = 0;
      while((data=(fis.read(b,0,b.length)))!=-1){
         sos.write(b,0,data);
      }
      sos.flush();      
   }catch(Exception e){
      out.print(e.toString());
   }finally{
      if(sos != null) sos.close();
      if(fis != null) fis.close();
   }
%>  
```

## 결과

![](../../../.gitbook/assets/2%20%2879%29.png)

* 첨부파일을 클릭하면 바로 다운로드가 일어난다.

