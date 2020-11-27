---
description: 2020.11.27 73일차
---

# 73 Days -

### ffffb사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5 - Spring
* 사용 서버 - WAS : Tomcat

## Spring

## Spring Container\(=엔진, API\) 유형

### 1. BeanFactory

![Spring Maven&#xC548;&#xC5D0;&#xC11C; &#xAD00;&#xB9AC;](../../.gitbook/assets/.png%20%2837%29.png)

* Bean을 관리해준다.

### 2. ApplicationContext

![](../../.gitbook/assets/applicationcontext.png)

* Bean을 관리해준다.

### 코드 : ListMainApp.java

```java
package com.mycompany.online;

import java.util.List;
import org.springframework.beans.factory.BeanFactory;
import org.springframework.beans.factory.xml.XmlBeanFactory;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import org.springframework.core.io.FileSystemResource;
import org.springframework.core.io.Resource;

public class ListMainApp {
	
	List<String> listList = null;
	   public void setListList(List<String> listList) {
	      this.listList = listList;
	   }
	   public static void main(String[] args) {
	      ListMainApp lma = new ListMainApp();
	      
	      //ApplicationContext사용시
	      ApplicationContext context = new ClassPathXmlApplicationContext("com\\\\mycompany\\\\online\\\\insaBean.xml");
	      ListController list2 = (ListController)context.getBean("insaBean");
	      for(String insa:list2.listBean) {
	    	  System.out.println(insa);
	      }
	      
	      //BeanFatory사용시
	      Resource resource = new FileSystemResource("C:\\workspace_sts3\\spring3\\src\\main\\java\\com\\mycompany\\online\\insaBean.xml");
	      BeanFactory factory = new XmlBeanFactory(resource);
	      ListController list = (ListController)factory.getBean("insaBean");
	      System.out.println(list.listBean);
	   }
}
```

### Console 출력

![ApplicationContext](../../.gitbook/assets/1%20%2879%29.png)

![BeanFactory](../../.gitbook/assets/2%20%2860%29.png)

