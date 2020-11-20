# sample코드 : 뉴스 목록 자동갱신

## sample코드 : 뉴스 목록 자동갱신

### 코드 : sample.jsp

```markup
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>자동 갱신 처리 - 샘플화면</title>
<%@ include file="/common/easyUI_common.jsp" %>
<!-- 좌표값으로 접근하기 위한 CSS 속성 position: absolute -->
<style type="text/css">
   div#d_news{
      position: absolute;
   }
</style>
<script type="text/javascript">
   function autoReload(){
      //alert("autoReload 호출");
      $.ajax({
          url:'jsonNewsList.jsp'
         ,type:'post'
         ,dataType:'html'
         ,success:function(data){
            //alert("서버에서 가져오는 뉴스 정보");
            $("#d_news").html(data);//html이라서 태그를 해석해 내용만 보여준다.
         },error:function(e){
            //$("#d_msg").show();
            $("#d_news").text(e.responseText);
         }
      });//////////////////////end of ajax
      //$("#d_news").css("left", "300px");
      //$("#d_news").css("top", "100px");
   }
</script>
</head>
```

* &lt;head&gt;영역
* &lt;div&gt;태그에 좌표로 접근 할 수 있는 속성을 부여해주는 CSS와  자동 갱신시 호출되는 함수가 생성되어 있다.
* ajax비동기 통신을 활용해 부분갱신을 처리한다.
* data를 화면에 바로 보여줘야 하므로 dataType은 html로한다.

```markup
<body>
<script type="text/javascript">
   $(document).ready(function(){
      function start(){
         watch = setInterval(autoReload, 5000)//함수이름, 시간정보ms
      }
      function stop(){
         setTimeout(function(){
            clearInterval(watch);
         }, 100000);
      }
      start();
      stop();//stop을 호출하지않으면 무한루프
   });
</script>
<h3>자동 갱신 페이지 구현</h3>
<!-- 뉴스 목록이 출력될 위치 (좌표값을 이용해 원하는 위치에 작업한다.-CSS) -->
<div id = "d_news" width="250px" height="50px">여기</div>
<div id = "d_msg" width="250px" height="50px"></div>
</body>
</html>
```

* &lt;body&gt;영역
* 시간간격별로 자동으로 autoReload함수를 실행하는 setInterval함수와  일정시간이 지나면 함수를 실행하는 setTimeout함수를 활용해 자동갱신을 구현했다. clearInterval함수가 지정된 자동갱신을 중지한다.
* 5초마다 갱신되고, 100초가 지나면 중지된다.
* 18번 &lt;div&gt;태그가 ajax로 받아온 data를 보여줄 자리

### 코드 : jsonNewsList.jsp

```java
<%@ page language="java" contentType="application/jsonl; charset=UTF-8"
    pageEncoding="UTF-8"%>
    
 <%!
    int x = 1;//멤버변수 - 디클러레이션
    public String newsList(String news[]){
       StringBuilder sb = new StringBuilder();
       sb.append("<table width='250px' border='1' borderColor='pink'>");
       sb.append("<tr><td>"+"제목 : "+news[0]+", "+"내용 : "+news[1]+", "
                           +"이름 : "+news[2]+"</td></tr>");
       sb.append("</table>");
       return sb.toString();
    }
 %>
```

* 디클러레이션 영역에서는 멤버변수를 사용 할 수 있다.
* 받아온 기본 data에 html태그를 붙이기위해 StringBuilder의 append함수를 사용했다. 이 메서드를 벗어나면 바로 html의 &lt;div&gt;태그에서 출력되어야 하므로 이 메서드 안에서 추가할 크기나, 색상 등을 정해준다. 
* &lt;table width='250px' border='1' borderColor='pink'&gt; &lt;tr&gt;&lt;td&gt;제목 : 제목1, 내용 : 내용1, 이름 : 기자1&lt;/td&gt;&lt;/tr&gt; &lt;/table&gt; 이 반환된다.
* data가 여러개일때, 이 메서드 안에서 append를 for문을 돌려버리면, 모든 data가 한번에 출력된다. - 멤버변수를 사용하는 이유, 이 함수의 파라미터를 살펴보면 문자 배열을 받아오는 것을 알 수 있다.

```java
 <%
    //스크립틀릿
    String news1[] = {"제목1", "내용1", "기자1"};
    String news2[] = {"제목2", "내용2", "기자2"};
    String news3[] = {"제목3", "내용3", "기자3"};
    String datas = " ";
    switch(x){
    case 1:
       datas = newsList(news1);
       x++;
       break;
    case 2:
       datas = newsList(news2);
       x++;
       break;
    case 3:
       datas = newsList(news3);
       x = 1;//다시 1번 뉴스 그룹으로 초기화
       break;
    }////////////////end of switch
    out.clear();
    out.print(datas);
 %>
```

* Sample코드이기 때문에 DB를 갓다오지는 않고, 상수로 값을 박아줬다.
* 출력하는 메서드에서 for문을 사용할 수 없기때문에 멤버변수를 사용해 경우를 구분한다.
* x=1이면 첫번째 요청이므로 첫번째 배열을 파라미터로 출력 메서드를 호출하고, x=2가된다.
* 그다음 요청은 두번째 배열을 넘기고, x=3이된다.
* 세번째 요청은 세번째 배열을 넘기고, x=1이된다.  data가 3개 이므로 4번째 자동갱신에서는 다시 첫번째 data가 출력되어야 할 것이다.
* break로 해당 case가 끝나면 switch문을 탈출하는 것을 잊지말자
* 21번에서 이전출력물을 깨끗이 삭제하고
* newsList에서 반환된 새로운 결과값을 내보낸다.

### 화면 1

![&#xC5EC;&#xAE30; = &#xAC12;&#xC774; &#xC704;&#xCE58;&#xD560; &#xC790;&#xB9AC;](../../../.gitbook/assets/1%20%2874%29.png)

### 화면2

![5&#xCD08; &#xD6C4;](../../../.gitbook/assets/2%20%2855%29.png)

### 화면3

![5&#xCD08; &#xD6C4;](../../../.gitbook/assets/3%20%2844%29.png)

### 화면4

![5&#xCD08; &#xD6C4;](../../../.gitbook/assets/4%20%2837%29.png)

* 5초가 지나면 다시 제목1, 내용1, 기자1 부터 반복되고, 100초가 지나면 자동갱신이 중지된다.

