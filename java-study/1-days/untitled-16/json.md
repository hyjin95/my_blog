# JSON 형식

## 올바르지 않은 형식

![](<../../../.gitbook/assets/4 (24).png>)

```markup
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ page import="java.util.List, java.util.Map" %>
<%@ page import="java.util.ArrayList, java.util.HashMap" %>
	
<%
	List<Map<String, Object>> testInfo = new ArrayList<>();
	Map<String, Object> rmap = new HashMap<>();
	rmap.put("code",2560);
	rmap.put("name","이성계");
	testInfo.add(rmap);
	rmap = new HashMap<>();
	rmap.put("code",3560);
	rmap.put("name","이성계");
	testInfo.add(rmap);
	rmap = new HashMap<>();
	rmap.put("code",5560);
	rmap.put("name","이성계");
	testInfo.add(rmap);
	out.print(testInfo);
%>
```

## 올바른 형식

![](<../../../.gitbook/assets/5 (17).png>)

```markup
<%@ page language="java" contentType="application/json; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ page import="java.util.List, java.util.Map" %>
<%@ page import="java.util.ArrayList, java.util.HashMap" %>
<%@ page import="com.google.gson.Gson" %>
	
<%
	List<Map<String, Object>> testInfo = new ArrayList<>();
	Map<String, Object> rmap = new HashMap<>();
	rmap.put("code",2560);
	rmap.put("name","이성계");
	testInfo.add(rmap);
	rmap = new HashMap<>();
	rmap.put("code",3560);
	rmap.put("name","이성계");
	testInfo.add(rmap);
	rmap = new HashMap<>();
	rmap.put("code",5560);
	rmap.put("name","이성계");
	testInfo.add(rmap);
	Gson g = new Gson();
	String temp = g.toJson(testInfo);
	out.print(temp);
%>
```

* mime타입 : application/json\
  \- text/html이여도 알아서 json으로 인식하기는 하지만 바꿔서 명시하도록한다.
