---
description: 2021.01.13 - 102일차
---

# 102 Days - 복습, Spring:HikariCP설정, Nexacro 연동한 전자정부 F/W Controller

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org, Toad DBA Suite for Oracle 11.5 , Spring, Android Studio, Visual Studio Code, Nexacro
* 사용 서버 - WAS : Tomcat

## 필기

### 값 유지

* 변수 - 배열\(고정, 상수, 같은타입\) - 객체배열\(유동\[List&lt;VO&gt;\], 다른타입까지도\) - List\(인덱스, 순서가 있어 속도가 느리다\) - Map\(중복가능, 순서가 업어 빠르다.\) - 시간을 관리하기 위해 쿠키와 세션을 사용한다.
* 주의 사항 - 쿠키는 반드시 페이지에 대한 요청이 있어야 반영된다.   값이 있더라도 브라우저가 새로고침\(주소창 변경\)되지 않으면 반영되지 않는다. \(ajax로는 안된다.\)   사용자의 local에 text로 저장된다. - 세션은 서버에 cashe로 관리되어 용량에 제한이 있다.

### Android에서의 쿠키와 세션

* 활용 불가능 : 네이티브앱 - 로컬 디바이스만 사용한다. - 하드웨어를 지원받아 활용할 수 있다.   카메라, 위치기반, 근거리 통신망, 비콘, 블루투스, QR 등.....
* 활용 가능 : 하이브리드 앱

### html의 form전송

* html내에서 form전송이 일어나면 요청의 변화로 기존의 은 리셋되고 새로운 돔구성이 발생한다.

### Bean

* spring-core.jst 스프링 기반의 프레임워크\(전자정부프레임워크, AnyFrameWork 등\)
* 빈 공장 운영 BeanFactory : spring-beans.jar ApplicationContext : spring-context.jar
* @Bean 어노테이션을 사용하거나 xml에 &lt;bean&gt;태그를 사용해 등록한다.

### DB연동, JDBC

* spring-jdbc.jar mybatis.jar mybatis-spring.jar
* mybaits-spring.jar에서 제공해주는 SqlSessionFactoryBean.java로 커넥션을 맺는다. SqlSessionTemplate.java에서는 selectOne, insert, delete, update, selectList 등을 지원한다.

### trigger

* 활성화 시키거나 비활성화 시키는 작업이지 호출해서 실행시키는 것이 아니다.
* rollback의 대상이 될 수 없다.

### Transaction

* spring-tx.jar spring-aop.jar
* 묶음배송

### Spring MVC

* spring-webmvc.jar spring-mvc.jar

### Spring 설정 방법

1. 자바 : 어노테이션 @Configuration 수동
2. XML으로 설정\(전자정부, 넥사크로\) 이종간 연동에서도 xxx.jar파일을 개발자가 직접 관리할 수 있다. 직접 찾아서 해야하고 의존관계를 찾아봐야 하므로 경험치가 필요하다.
3. Boot로 설정 자동 이종간의 의존성주입에서 트러블이 발생할 가능성이 높다. spring만 사용할때에는 boot를 활용하는것이 편하다.

## Spring : HikariCP

### Maven Repository

* HikariCP 3.4.5
* spring-jdbc ${org.springframework-version
* mybatis 3.5.6
* mybatis-spring 2.0.6

### WEB-INF &gt; spring-data.xml

```markup
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
	
	<!-- 
	=============================================================================================================
		HikariCP DataSource
	=============================================================================================================
	 -->
	<bean id="data-source-target" class="com.zaxxer.hikari.HikariDataSource" destroy-method="close">
		<constructor-arg ref="hikariConfig"/>
	</bean>
	
	<!-- 
	=============================================================================================================
		HikariCP Configuration
	=============================================================================================================
	 -->
	<bean id="hikariConfig" class="com.zaxxer.hikari.HikariConfig">
		<property name="driverClassName">
			<value>oracle.jdbc.driver.OracleDriver</value>
		</property>
		<property name="jdbcUrl">
			<value>jdbc:oracle:thin:@192.168.0.187:1521:orcl11</value>
		</property>
		<property name="username">
			<value>scott</value>
		</property>
		<property name="password">
			<value>tiger</value>
		</property>
		<property name="autoCommit">
			<value>false</value>
		</property>
	</bean>
	
	<!-- myBatis를 사용할 수 있도록 spring에서 sqlSessionFactory를 제공한다. -->
	<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean"><!-- driver class이름 -->
		<property name="configLocation" value="WEB-INF/mybatis-config.xml"/>
		<property name="dataSource" ref="data-source-target"/>
	</bean>
	
	<!-- myBatis를 사용할 수 있도록 spring에서 sqlSessionTemplate=sqlSesion를 제공한다. 위 bean과 의존관계에 있다. -->
	<bean id="sqlSessionTemplate" class="org.mybatis.spring.SqlSessionTemplate">
		<constructor-arg index="0" ref="sqlSessionFactory"/>
	</bean>

</beans>	
```

## Nexacro DB 연동 Controller

### RestDeptController

```java
package com.mvc.dept;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

import javax.servlet.http.HttpServletRequest;

import org.apache.log4j.Logger;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;

import com.google.gson.Gson;
import com.nexacro17.xapi.data.DataSet;
import com.nexacro17.xapi.data.DataTypes;
import com.nexacro17.xapi.data.PlatformData;
import com.nexacro17.xapi.tx.HttpPlatformRequest;
import com.nexacro17.xapi.tx.PlatformException;

@Controller
@RequestMapping("/dept/*")
public class RestDeptController {
	Logger logger = Logger.getLogger(RestDeptController.class);	
	
	@GetMapping(value="deptXML.kosmo",produces="application/json; charset=UTF-8")
	public void deptXML(HttpServletRequest req) throws PlatformException {
		logger.info("deptXML 호출성공");
		
		//넥사크로에서 요청 받아오기
		HttpPlatformRequest nreq = new HttpPlatformRequest(req);//넥사에서 제공하는 요청객체
		nreq.receiveData();//데이터를 가져오는 메서드 호출
		//넥사크로에서 제공하는 데이터 셋에 담긴 정보를 자바코드에서 읽어오기 위한 선언
		PlatformData in_ndata = nreq.getData();
		//DataSet : 넥사크로에서 제공하는 데이터셋을 자바코드로 제공하는 클래스이다.
		DataSet ids_dept = in_ndata.getDataSet("in_dept");//화면에서 넘어온 데이터셋을 읽기위한 코드
		//자바를 이용해 오라클 서버에서 select한 결과를 담을 데이터셋 선언
		DataSet ods_dept = new DataSet("out_dept");//화면에 내보낼 정보
		//데이터셋 초기화 - 자바
		ods_dept.addColumn("deptno", DataTypes.INT, (short)10);
		ods_dept.addColumn("dname", DataTypes.STRING, (short)10);
		ods_dept.addColumn("loc", DataTypes.STRING, (short)10);
		
		List<Map<String,Object>> deptList = null;
		for(int i=0;i<deptList.size();i++) {
			Map<String,Object> rmap = deptList.get(i);
			//데이터 셋에 로우를 추가해 주어야 조회된 결과를 담을 수 있다.
			int row = ods_dept.newRow();
			ods_dept.set(row,"deptno",rmap.get("deptno"));
			ods_dept.set(row,"dname",rmap.get("dname"));
			ods_dept.set(row,"loc",rmap.get("loc"));
		}
	}
}
```

