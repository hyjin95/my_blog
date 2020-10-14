# MyBatis API - Cinfig.properties

## Configuration.XML

```markup
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration
 PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
 "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
	<properties resource = "com/util/Config.properties"/>
	<environments default="development">
		<environment id="development">
			<transactionManager type="JDBC" />
			<dataSource type="POOLED">
				<property name="driver" value="${driver}" />
				<property name="url" value="${url}" />
				<property name="username" value="${username}" />
				<property name="password" value="${password}" />
			</dataSource>
		</environment>
	</environments>
	<mappers>
		<mapper resource="oracle/mybatis/zipcode.xml"/>
	</mappers>
</configuration>
```

* MyBatis API가 제공하는 기본 XML설정 소스

## Config.properties

```markup
#주석입니다.
driver = oracle.jdbc.driver.OracleDriver
url = jdbc:oracle:thin:@192.168.0.187:1521:orcl11
username = scott
password = tiger
```

## MyBatisCommonFactory.java

```java
package com.util;

import java.io.FileNotFoundException;
import java.io.IOException;
import java.io.Reader;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
//xml과 자바가 만나는 부분 설계 클래스 --공통팀
public class MyBatisCommonFactory {
	//선언부
	//xml문서로부터 객체를 주입받아야 하므로 단독으로 인스턴스화해버리면 안된다.
	public SqlSessionFactory sqlSessionFactory = null;
	public SqlSession		 session = null;//오라클에 DML을 요청하는 클래스
	//위 두 클래스는 서로 의존관계에 있다. 연결통로 생성 -> 요청클래스 생성이 이루어져야한다.
	
	//생성자
	
	//초기화
	public void init() {
		try {
			//MapperConfig.xml문서에서 오라클 서버에 대한 정보를 스캔해야한다.--1
			//물리적으로 떨어져있는 오라클과의 연결통로를 생성해야한다. = SqlSessionFactory(myBatis제공 클래스) --2
			//String resource ="oracle/mybatis/MapperConfig.xml";			
			String resource ="com/util/Configuration.xml";			
			Reader reader = Resources.getResourceAsReader(resource);
			//builder클래스로 sqlSessionFactory를 인스턴스화
			sqlSessionFactory = new SqlSessionFactoryBuilder().build(reader,"development");//Factory클래스가 먼저 생성되어야한다.		
			System.out.println("SqlSessionFactory(SqlSessionFactoryBean) "+sqlSessionFactory);//객체주입이 되었는지 확인하는 단위테스트, null이 나오면 안된다.
		} catch (FileNotFoundException fe) {
			System.out.println("FileNotFoundException : "+fe.getMessage());
		} catch (IOException ie) {
			System.out.println("IOException : "+ie.getMessage());
		} catch(Exception e) {
			System.out.println("Exception : "+e.getMessage());
		}
	}
	//싱글톤 패턴으로 개발을 전개해야 할 때는 메서드로 객체주입 받도록 한다.
	public SqlSessionFactory getSqlSessionFactory() {
		init();//sqlSessionFactory의 객체 생성이 일어난다.
		return sqlSessionFactory;//init메서드를 거치지 않으면 null
	}
}
```

