---
description: 2020.11.12 - 62일차
---

# 62 Days -

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5
* 사용 서버 - WAS : Tomcat

### 반복 getParameter : 공통코드 구현

```java
HashMapBinder class

public void bind(Map< , > target){
    target.clear( );
    Enumeration en = req.getParameterNames( );
    while(en.hashMoreElement()){
    
    }
}
```

* Enumeration은 판정기능을 갖고 있다.
* map.put\( \), list.add\( \)를 사용해 n개의 name을 담는다.

