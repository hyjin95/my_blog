# API활용 - easyui-textbox : Enter Event구현

## empManagerHtype - &lt;body&gt;

```markup
<body>
<script type="text/javascript">   
   $(document).ready(function (){	  
	  //2.콤보박스 초기화 및 설정
	  $("#cb_search").combobox({
		  data:[
			   {cols:'empno', label:'사원번호'}
			  ,{cols:'ename', label:'사원이름'}
			  ,{cols:'sal'  , label:'급여'}
		  ]
		 //rec : 콤보박스에서 선택했을떄 @param:object로서 row의 주소번지를 갖는다.
		 //row.empno, row.ename
	  	 ,onSelect:function(rec){
	  		alert('당신이 선택한 검색조건은 '+rec.cols);
			test=rec.cols;
	  	 }
	  });	   
      //1.datagrid에 대한 초기화
	   $("#dg_emp").datagrid({
            toolbar:'#tb_emp'
            ,singleSelect:true
        });
	   $('#tb_keyword').textbox('textbox').bind('keydown', function(e){
		   	if (e.keyCode == 13){	// when press ENTER key, accept the inputed value.
		   		empSearch2();
		   	}
		   });
   });
</script>
<!-- 이 페이지에 대한 에러 메시지나 힌트문을 출력하자. -->
<div id="d_msg"></div>
<!------------------------ 툴바 추가 시작 --------------------------->
<div id="tb_emp">
<table border="0" width="100%">
	<tr>
		<td>
			<table>
					<!-- combobox추가 (위치선택 -> 공간확보 -> 코드작성,추가) 이름|job|부서번호 -->
				<tr>
					<!--------------------추가 ----------------------->
					<td width="1000px" colspan="3">
						<label width="100px">검색분류 : </label>
						<input id="cb_search" class="easyui-combobox" data-options="
        					valueField: 'cols',
        					textField: 'label'
        					">
						<input id="sb_keyword" name="sb_keyword" class="easyui-searchbox" style="width:150px" 
							   data-options="searcher:empSearch3,prompt:'검색할 값 입력'"></input>
						<input id="tb_keyword" name="tb_keyword" class="easyui-textbox" style="width:150px" onclick='empSearch3()'
							   data-options="prompt:'검색할 값 입력'"></input>
        					
					</td>					
					<!-------------------- 끝 ----------------------->
				</tr>
			</table>			
		</td>		
	</tr>
</table>
</div>
</body>
```

## empManagerHtype - &lt;head&gt;

### empSearch\( \) - searchbox

```markup
<head>
<meta charset="UTF-8">
<title>사원관리(empManagerDType.html)</title>
<script type="text/javascript">

     function empSearch(){        	 
         $("#dg_emp").datagrid({            	
        	url:'../../getEmpList2.jsp?cols='+$("#cb_search").val()+"&keyword="+$("#sb_keyword").val()//name을 쿼리스트링으로 넘긴다.
        	,onLoadSuccess:function(temp){            		
        		var result = JSON.stringify(temp);   
   			 		alert("onload....."+result);
        		var test = result.split('"rows":',2);
            var test2 = test[1].substring(0,test[1].length-1);
   			 		alert("onload....."+test2); 
   			 		var result2 = JSON.parse(test2);
   			 		alert("onload....."+result2[0].ENAME);    			 		
   			 	}		
         });
     }
</script>
</head>
```

### empSearch2\( \) - textbox

```markup
<head>
<meta charset="UTF-8">
<title>사원관리(empManagerDType.html)</title>
<script type="text/javascript">
   
     function empSearch2(){
         $("#dg_emp").datagrid({            	
        	url:'../../getEmpList2.jsp?cols='+$("#cb_search").val()+"&keyword="+$("#tb_keyword").val()//name을 쿼리스트링으로 넘긴다.
        	,onLoadSuccess:function(temp){            		
            	var result = JSON.stringify(temp);   
            	var test = result.split('"rows":',2);
            	var test2 = test[1].substring(0,test[1].length-1);
   			 		  var result2 = JSON.parse(test2);
   			 	}		
         });
     }
</script>
</head>
```

### empSearch3\( \) - 합치기, if

```markup
<head>
<meta charset="UTF-8">
<title>사원관리(empManagerDType.html)</title>
<script type="text/javascript">  
         
     function empSearch3(){
        	var keyword;//사용자가 입력한 값 담기
        	if($("#tb_keyword").val().length>0){
        	  keyword=$("#tb_keyword").val();
        	 }
        	if($("#sb_keyword").val().length>0){
        		 keyword=$("#sb_keyword").val();
        	}
        	$("#d_msg").append(tb_keyword+", "+sb_keyword+", "+keyword+"<br>");
             $("#dg_emp").datagrid({            	
            	 url:'../../getEmpList2.jsp?cols='+$("#cb_search").val()+"&keyword="+keyword//get방식 쿼리스트링
            	 //url:'../../getEmpList2.jsp?cols='+$("#cb_search").val()+"&keyword="+keyword+"&timestamp="+new Date().getTime()//초단위로 찍어줘 서버까지 전달된다.
            	,method:'post'//
            	,onLoadSuccess:function(temp){            		
            		var result = JSON.stringify(temp);   
            		var test = result.split('"rows":',2);
            		var test2 = test[1].substring(0,test[1].length-1);
   			 		    var result2 = JSON.parse(test2);
   			    	}		
             });
            //초기화
        	 keyword='';
        	 $("#tb_keyword").textbox('setValue','');
        	 $("#sb_keyword").searchbox('setValue','');
         }
</script>
</head>
```

