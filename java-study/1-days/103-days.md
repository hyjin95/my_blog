---
description: 2021.01.14 - 103일차
---

# 103 Days -

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org, Toad DBA Suite for Oracle 11.5 , Spring, Android Studio, Visual Studio Code, Nexacro
* 사용 서버 - WAS : Tomcat

## Nexacro

```javascript
this.divCommand_btnSearch_onclick = function(obj:nexacro.Button,  e:nexacro.ClickEventInfo)
{
    
     var id = "search";  
     var url = "http://localhost:8080/CustomerList/sample.xml";
     var reqDs = "";
     var respDs = "dsCustomers=customers";
     var args = "";
     var callback = "received";
    
     this.transaction(id, url, reqDs, respDs, args, callback);    
}
```

* id  - 트랜잭션 구분 식별자
* url  - 트랜잭션을 요청하는 서비스, 파일의 URL - 쿼리스트링을 붙여 get방식으로 값을 넘길 수 있다.
* reqDs - Request Data Set - \(tomcat\)서버의 요청을 기준으로 화면에서 서버로 전송할 값 - \(컨트롤러에서 사용할 변수\[값, 데이터셋\]\) = \(넥사크로 스튜디오에 지정된 값\[데이터 셋\]\)
* args
* callbask

