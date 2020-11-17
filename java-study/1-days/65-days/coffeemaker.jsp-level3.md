# coffeeMaker.jsp : Level3 - 두 개의 요청객체

## coffeeMaker.jsp : View

```markup
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
   String coffeemaker = request.getParameter("coffeemaker");
   String name = request.getParameter("name");
   for(double i=0;i<900000000.0;i++){
      //커피를 만드는 시늉을 한다.
   }
   //기존에 갖고 있던 정보를 출력버퍼에서 삭제하기
   //이걸 안하면 계속 1번 머신 정보만 유지될 수도 있다.
   out.clear();
   out.print(coffeemaker+name);
%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Ajax-powered Coffee Maker</title>
	<script type="text/javascript" src="coffee.js?12"></script>
    <link rel="stylesheet" type="text/css" href="coffee.css?12"/>
<script language="javascript">
	//요청을 받을 수 잇는 커피머신이 두개이므로 스레드를 두개 주는 것과 같이 요청객체를 두개 만든다.
   var request1 = createRequest();
   var request2 = createRequest(); 
  </script>
 </head>
 <body>
   <div id="header">
    <h1>Ajax-powered Coffee Maker</h1>
   </div>
  <div id="wrapper">
   <div id="coffeemaker1">
    <h2>Coffee Maker #1</h2>
    <p><img src="../../images/CoffeeMaker1.gif" alt="Coffee Maker #1" /></p>
    <div id="coffmaker1Status">Idle</div>
   </div>
   <div id="coffeeorder">
    <p><img src="../../images/coffeeMugWithBeans.jpg" alt="Coffee Pot 1" /></p>
    <h2>Place your coffee order here:</h2>
    <div id="controls1">
     <form action="#">
      <p>Name: <input type="text" name="name" id="name" /></p>
      <h3>Size</h3>
      <p> 
       <input type="radio" name="size" 
              value="small" checked="true">Small</input>&nbsp;&nbsp;
       <input type="radio" name="size" value="medium">Medium</input>&nbsp;&nbsp;
       <input type="radio" name="size" value="large">Large</input>
      </p>
      <h3>Beverage</h3>
      <p> 
       <input type="radio" name="beverage" 
              value="mocha" checked="true">Mocha</input>&nbsp;&nbsp;
       <input type="radio" name="beverage" 
              value="latte">Latte</input>&nbsp;&nbsp;
       <input type="radio" name="beverage" 
              value="cappucino">Cappucino</input>
      </p>
      <p>
       <input type="button" onClick="orderCoffee()" value="Order Coffee" />
      </p>
     </form>
    </div>
   </div>
   <div id="coffeemaker2">
    <h2>Coffee Maker #2</h2>
    <p><img src="../../images/CoffeeMaker2.gif" alt="Coffee Maker #2" /></p>
    <div id="coffeemaker2-status">Idle</div>
   </div>
   <p id="clear"></p>
  </div>
 </body>
</html>
```

## coffee.css : CSS

```css
@charset "UTF-8";
@CHARSET "UTF-8";
body {
  font-family: "Lucida Grande", Verdana, Arial, sans-serif;
  text-align: center;
  font-size: small;
  margin: 10px;
  background-color: silver;
}

h1 {
  font-size: 140%;
}

h2 {
  font-size: 120%;
  padding-top: 10px;
}

h3 {
  font-size: 100%;
  text-decoration: underline;
}

p {
  margin-bottom: 20px;
}

img {
  height: 100px;
  padding: 10px;
}

form {
  margin-top: 20px;
}

div#wrapper {
  position: absolute;
  width: 746px;
  left: 410px;
  margin: 10px auto;
  padding: 10px;
  color: #333;
  background-color: white;
  border: 1px solid black;
}

div#coffeeorder {
  position: absolute;
  top: 20px;
  left: 240px;
  width: auto;
  margin: 0;
  padding: 10px;
}

div#coffeemaker1 {
  position: absolute;
  top: 10px;
  padding: 10px;
  width: 200px;
  background-color: #f5f5f5;
  border:double medium black;
  left: 10px;
}

div#coffeemaker2 {
  position: absolute;
  top: 10px;
  padding: 10px;
  width: 200px;
  background-color: #f5f5f5;
  border:double medium black;
  left: 530px;
}

#clear {
  clear: both;
}
```

## coffee.js : JS

```javascript
   function createRequest(){
       var xhrObject = null;
        try{
            xhrObject = new XMLHttpRequest();
        }catch(trymicrosoft){
            xhrObject = null;
        }
        if(xhrObject==null){
            alert("비동기 통신객체 생성 실패");
        }else{
           return xhrObject;
        }
    } 
     function getSize(){
		//document.forms[0] document가 가르키는 html의form태그 중 첫번째
        var sizeGroup = document.forms[0].size;// => 3
        for(var i=0;i<sizeGroup.length;i++){
          if(sizeGroup[i].checked==true){
             return sizeGroup[i].value;
          }
        }        
     }        
     function getBeverage(){
        var beverageGroup = document.forms[0].beverage;// => 3
        for(var i=0;i<beverageGroup.length;i++){
          if(beverageGroup[i].checked==true){
             return beverageGroup[i].value;
          }
        }         
     }
     function sendRequest(xhrObject, url){
        xhrObject.onreadystatechange = serveDrink;
        xhrObject.open("GET",url,true);
        xhrObject.send(null);
     }
     function serveDrink(){
        alert("serveDrink호출");
        if(request1.readyState == 4){
           if(request1.status == 200){
              //응답 받아오기 ==> 1김유신 xml이 아닌 text이다.
              var res = request1.responseText;
              var machine = res.substring(0,1);//1
              var name = res.substring(1,res.length);
              if(machine == "1"){
                 //주문한 커피가 나왔으므로 머신 상태를 Idle로 변경함.
                   var coffeemaker1 = document.getElementById("coffmaker1Status");
                   replaceText(coffeemaker1,"Idle");//Idle 놀고있는 상태         
              }
              else{//2번 머신인 경우
                   var coffeemaker2 = document.getElementById("coffeemaker2-status");
                   replaceText(coffeemaker2,"Idle");                     
              }
              alert(name+", your coffee is ready!");
              request1 = createRequest();
           }/////////////end of 200
           else{
              alert("Error"+ request1.status);
           }
        }/////////////////end of 4
        else if(request2.readyState == 4){
           if(request2.status == 200){
              //응답 받아오기 ==> 2애플
              var res = request2.responseText;
              var machine = res.substring(0,1);//1
              var name = res.substring(1,res.length);  
              if(machine == "1"){
                   var coffeemaker1 = document.getElementById("coffmaker1Status");
                   replaceText(coffeemaker1, "Idle"); 

              }else{
                   var coffeemaker2 = document.getElementById("coffeemaker2-status");
                   replaceText(coffeemaker2, "Idle");                   
              }
              alert(name+", your coffee is ready!");
              request2 = createRequest();
           }
           else{
              alert("Error"+ request2.status);
           }
        }
     }
     function orderCoffee(){
        //주문자의 이름 정보 출력
        var name = document.getElementById("name").value;
        var size = getSize();
        var beverage = getBeverage();
        alert(name+", "+size+", "+beverage);   
        //insert here
        //첫번째 머신의 아이디값 가져오기
        var coffeemaker1 = document.getElementById("coffmaker1Status");
        var status = getText(coffeemaker1);
        if(status=="Idle"){
           //insert here - 누구님 컵사이즈, 커피를 준비중입니다.
           replaceText(coffeemaker1
                    ,name+"님의 "+beverage+"("+size+")"+"를 준비중입니다.");
           //커피 주문서에 작성된 내용을 초기화 함.
           document.forms[0].reset();
           //커피를 내리는 시늉을 하는 jsp페이지의 url정보를 담음.
           var url = "coffee.jsp?coffeemaker=1&name="+name+"&timestamp="+new Date().getTime();
           sendRequest(request1,url);
        }
        else{
           //두번째 머신의 아이디값 가져오기
           var coffeemaker2 = document.getElementById("coffeemaker2-status");
           var status = getText(coffeemaker2);
           if(status=="Idle"){
              replaceText(coffeemaker2
                       ,"Brewing "+ name+"'s "+size+" "+beverage);
              //두번째 주문이 처리가 된 경우임.
              document.forms[0].reset();
              var url = "coffee.jsp?coffeemaker=2&name="+name+"&timestamp="+new Date().getTime();
              sendRequest(request2,url);
           }else{//두 대가 다 일하는 경우
              alert("Sorry! Both coffee makers are busy. \n"
                  +"Try again later.");
           }
        }
     }
     function getText(el){//el => $('#id')
         var text="";
         if(el!=null){//태그가 존재하지 않으면
             if(el.childNodes){//태그안에 여러개 노드가 있으면 컬렉션으로 담음
                 for(var i=0;i<el.childNodes.length;i++){
                     //여러개 노드중 한개 - 700000
                     var childNode = el.childNodes[i];
                     //너 텍스트 노드니?
                     if(childNode.nodeValue!=null){//700000!=null
                         text = text+childNode.nodeValue;
                     }
                 }
             }
         }
         return text;
     }  
     function clearText(el){
         if(el!=null){
             if(el.childNodes){//0만 false
                 for(var i=0;i<el.childNodes.length;i++){
                     var childNode = el.childNodes[i];
                     el.removeChild(childNode);
                 }
             }
         }
     }     
     function replaceText(el,text){
         if(el!=null){
             //기존 노드에 들어있는 값은 초기화
             clearText(el);
             //새로운 텍스트 노드를 추가
             var newNode = document.createTextNode(text);//12
             el.appendChild(newNode);
         }
     }     
```

## coffee.jsp : JSP - 요청처리

```java
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
	//커피머신의 번호가 넘어온다.
   String coffeemaker = request.getParameter("coffeemaker");
	//커피 주문자 이름이 넘어온다.
   String name = request.getParameter("name");
   for(double i=0;i<90000000000.0;i++){
      //커피를 만드는 시늉을 한다. 시간을 번다.
   }
   //기존에 갖고 있던 정보를 출력버퍼에서 삭제하기
   //이걸 안하면 계속 1번 머신 정보만 유지될 수도 있다.
   out.clear();//정보 초기화
   out.print(coffeemaker+name);
%>
```

