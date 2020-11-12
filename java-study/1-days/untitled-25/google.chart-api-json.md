# google.chart API - \[\[ \]\], JSON\[{ }\]

## lineChart.jsp - google.Chart API살펴보기

```markup
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
 <%
 	//MyBatis를 활용해 조회해보자
 	//emp : 사원번호, 급여, 인센티브를 가져와 차트에 뿌려보자
 %>
<!DOCTYPE html>
<html>
  <head>
    <script type="text/javascript" src="https://www.gstatic.com/charts/loader.js"></script>
    <script type="text/javascript">
      google.charts.load('current', {'packages':['corechart']});
      google.charts.setOnLoadCallback(drawChart);

      function drawChart() {
        var data = google.visualization.arrayToDataTable([
          ['Year', 'Sales', 'Expenses']
          
          for(int i=0; i<3, i++){

        	  ,['<%="2004"%>', <%= 1000%>, <%= 400%>]
          }
          //['2004',  1000,      400],
          //['2005',  1170,      460],
          //['2006',  660,       1120],
          //['2007',  1030,      540]
        ]);

        var options = {
          title: 'Company Performance',
          curveType: 'function',
          legend: { position: 'bottom' }
        };

        var chart = new google.visualization.LineChart(document.getElementById('curve_chart'));

        chart.draw(data, options);
      }
    </script>
  </head>
  <body>
    <div id="curve_chart" style="width: 700px; height: 400px"></div>
  </body>
</html>
```

* 보면 15번 drawChart메서드안에서 DataTable의 형식이 일반 JSON형식이 아님을 알 수 있다. \[ \[ '컬럼명', '컬럼명' \] , \[ '값', 값 \] \]

## Step 1 : lineChart2.jsp - Ajax와 JSON

```markup
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>

<!DOCTYPE html>
<%
 	//MyBatis를 활용해 조회해보자
 	//emp : 사원번호, 급여, 인센티브를 가져와 차트에 뿌려보자
 %>
<html>
  <head>
    <link rel="stylesheet" type="text/css" href="https://www.jeasyui.com/easyui/themes/default/easyui.css">
    <link rel="stylesheet" type="text/css" href="https://www.jeasyui.com/easyui/themes/icon.css">
    <link rel="stylesheet" type="text/css" href="https://www.jeasyui.com/easyui/themes/color.css">
    <link rel="stylesheet" type="text/css" href="https://www.jeasyui.com/easyui/demo/demo.css">
    <script type="text/javascript" src="https://www.jeasyui.com/easyui/jquery.min.js"></script>
    <script type="text/javascript" src="https://www.jeasyui.com/easyui/jquery.easyui.min.js"></script>
    
    <script type="text/javascript" src="https://www.gstatic.com/charts/loader.js"></script>
    <script type="text/javascript">
      google.charts.load('current', {'packages':['corechart']});
      google.charts.setOnLoadCallback(drawChart);
		
      //dataType을 text로 한다면 stringify로 전부 잘라야 사용할 수 있다.
      //data를 비동기적으로 호출해보자
      //JSON형태이므로 까일것이다. [{ }]
      function drawChart() {
    	  
    	  $.ajax({
    		  url:'./jsonEmpList.jsp' 
    		 ,dataType:'json' 
    		 ,success:function(data){ //data = Object
    			 var result = JSON.stingify(data);//스트링으로
    			 var docArr = JSON.parse(reault);//배열로 arry
    			 for(var i=0; r<docArr.length;i++){
    				 var v_empno = docArr[i].EMPNO;//MyBatis를 거치면 대문자
    				 var v_sal   = docArr[i].SAL;
    				 var v_comm  = docArr[i].COMM;
    			 }
    		 }
    	  });
 
        var data = google.visualization.arrayToDataTable([//JSON형태가 아니다.[[ ]]
          ['Year', 'Sales', 'Expenses']
         ,document.write();//하면 되지 않을까? 안된다 기존화면을 다 갈아엎는 행위가 되므로
<%          
          for(int i=0; i<3, i++){
%>
        	  ,['<%="2004"%>', <%= 1000%>, <%= 400%>]
<%
          }
%>
        ]);

        var options = {
          title: 'Company Performance',
          curveType: 'function',
          legend: { position: 'bottom' }
        };

        var chart = new google.visualization.LineChart(document.getElementById('curve_chart'));

        chart.draw(data, options);
      }
    </script>
  </head>
  <body>
    <div id="curve_chart" style="width: 700px; height: 400px"></div>
  </body>
</html>
```

* 가져온 data가 JSON형식이기 떄문에 출력 되지 않는다. 
* 차트 DataTable이 인식하지 못한다.

## Step 2 : lineChart3 - Map&lt;String&gt;으로 꽂는다.

```markup
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ page import="java.util.List, java.util.Map" %>
<%@ page import="java.util.ArrayList" %>
<%@ page import="com.google.gson.Gson" %>
<%@ page import="web.mvc.EmpDao" %>
<!DOCTYPE html>
<%
 	//MyBatis를 활용해 조회해보자
 	//emp : 사원번호, 급여, 인센티브를 가져와 차트에 뿌려보자
	int size=0;
	EmpDao eDao = new EmpDao();
	List<Map<String,Object>> empList = new ArrayList<>();
	empList = eDao.getEmpList(null);
	if(empList!=null){
		size = empList.size();//미리 사이즈를 뽑아놓으면 for문 사용시 편리
	}

 %>
<html>
  <head>
    <link rel="stylesheet" type="text/css" href="https://www.jeasyui.com/easyui/themes/default/easyui.css">
    <link rel="stylesheet" type="text/css" href="https://www.jeasyui.com/easyui/themes/icon.css">
    <link rel="stylesheet" type="text/css" href="https://www.jeasyui.com/easyui/themes/color.css">
    <link rel="stylesheet" type="text/css" href="https://www.jeasyui.com/easyui/demo/demo.css">
    <script type="text/javascript" src="https://www.jeasyui.com/easyui/jquery.min.js"></script>
    <script type="text/javascript" src="https://www.jeasyui.com/easyui/jquery.easyui.min.js"></script>
    
    <script type="text/javascript" src="https://www.gstatic.com/charts/loader.js"></script>
    <script type="text/javascript">
      google.charts.load('current', {'packages':['corechart']});
      google.charts.setOnLoadCallback(drawChart);
		
      //dataType을 text로 한다면 stringify로 전부 잘라야 사용할 수 있다.
      //data를 비동기적으로 호출해보자
      //JSON형태이므로 까일것이다. [{ }]
      function drawChart() {
	     var data = google.visualization.arrayToDataTable([//JSON형태가 아니다.[[ ]]
          	['Year', 'Sales', 'Expenses']
<%          
          for(int i=0; i<size; i++){
        	  Map<String,Object> rmap = empList.get(i);
%>
        	  ,['<%=rmap.get("EMPNO").toString()%>', <%= rmap.get("SAL")%>, <%= rmap.get("COMM")%>]
<%
          }
%>
        ]);

        var options = {
          title: 'Company Performance',
          curveType: 'function',
          legend: { position: 'bottom' }
        };

        var chart = new google.visualization.LineChart(document.getElementById('curve_chart'));

        chart.draw(data, options);
      }
    </script>
  </head>
  <body>
    <div id="curve_chart" style="width: 700px; height: 400px"></div>
  </body>
</html>
```

* JS는 절차 지향적 언어이기때문에 31-33번의 차트를 그리는 콜백메서드 호출 이전에 size가 정해져야 한다.

## jsonEmpList.jsp 

```java
<%@ page language="java" contentType="application/json; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ page import="java.util.List, java.util.Map" %>
<%@ page import="java.util.ArrayList" %>
<%@ page import="com.google.gson.Gson" %>
<%@ page import="web.mvc.EmpDao" %>
<%
	//SELECT empno, sal, comm FROM emp 
	//comm이 테이블에서 null인 경우도 있어 nullPointerException이 발생할 수 있다.
	//SELECT empno, sal, NVL(comm,0) as "comm" FROM emp
	
	EmpDao eDao = new EmpDao();
	List<Map<String,Object>> empList = null;
	empList = eDao.getEmpList(null);//전체를 뽑아올 거라 파라미터가 없어도됨
	//out.print(empList);//이름에 " "이 없는 값이 나올 수 있다.JSON은 스트링인 경우 " "가 있어야 하는데 없으면 변수취급하므로
	
	Gson g = new Gson();
	String imsi = g.toJson(empList);//JSON형식으로 변환하여 이름을 변수가아닌 문자취급하도록한다.
	out.print(imsi);	
%>
```

