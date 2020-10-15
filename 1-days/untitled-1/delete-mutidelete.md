# Delete, mutidelete

## SqlMapDeptDao.java

### 선언부

```java
public class SqlMapDeptDao {
  //xml문서가 물리적으로 어디에 있는지를 알아야 한다.
	String resource ="oracle/mybatis/MapperConfig.xml";
	//myBatis에서 지원하는 클래스로
	SqlSessionFactory sqlMapper = null;
	
	Logger logger = Logger.getLogger(SqlMapDeptDao.class);
	
	MyBatisCommonFactory mcf = new MyBatisCommonFactory();
```

### 생성자

```java
public SqlMapDeptDao() {
		sqlMapper = mcf.getSqlSessionFactory();
	}
```

### deptDelete 메서드

```java
public int deptDelete(int value) {
		int result = 0;
		SqlSession session = sqlMapper.openSession();	
		try {
			//value = 99;
			result = session.delete("deptDelete", value);			
			logger.info("result : "+result);				
		} catch (Exception e) {
			System.out.println(e.toString());
		} finally {
			session.close();
		}
		return result;
	}////////////////////deptDelete////////////////////////
```

### deptDelete2 메서드

```java
public int deptDelete2(DeptVO dVO) {
		int result = 0;
		SqlSession session = sqlMapper.openSession();	
		try {
			//dVO= new DeptVO();
			//dVO.setDeptno(50);
			result = session.delete("deptDelete2", dVO);			
			logger.info("result : "+result);			
		} catch (Exception e) {
			System.out.println(e.toString());
		} finally {
			session.close();
		}
		return result;
	}/////////////////////deptDelete/////////////////////
```

### deptDelete3 메서드

```java
public int deptDelete3(Map<String, Object> map) {
		int result = 0;
		SqlSession session = sqlMapper.openSession();		
		try {
			//map = new HashMap();
			//map.put("deptno", 60);
			result = session.delete("deptDelete3", map);			
			logger.info("result : "+result);				
		} catch (Exception e) {
			System.out.println(e.toString());
		} finally {
			session.close();
		}
		return result;
	}/////////////////deptDelete///////////////////
```

### multiDeptDelete

```java
public int multiDeptDelete(String[] deptnos) {
		int result = 0;
		SqlSession session = sqlMapper.openSession();
		try {
			result = session.delete("multiDeptDelete", deptnos);	
			logger.info("result : "+result);				
		} catch (Exception e) {
			System.out.println(e.toString());
		} finally {
			session.close();
		}
		return result;
	}///////////////////multiDeptDelete///////////////////////
```

## Dept.XML

```markup
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="oracle.mybatis.DeptMapper">

 <delete id="deptDelete" parameterType="int">
   delete from dept where deptno=#{value} <!-- int이므로 값=value -->
 </delete>
 
 <delete id="deptDelete2" parameterType="com.vo.DeptVO">
   delete from dept where deptno=#{deptno} <!-- DeptVO의 멤버변수(get,set으로 구현된) -->
 </delete>
 
 <delete id="deptDelete3" parameterType="map">
   delete from dept where deptno=#{deptno} <!-- map이므로 key값 -->
 </delete>
 
 <delete id="multiDeptDelete" parameterType="list">
   delete from dept where deptno in 
   <foreach open="(" close=")" collection = "array" item="item" index="index" separator=",">
 		#{item}
   </foreach>
 </delete>
</mapper>
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
	//한 번 생성한 후 서버가 유지되는 동안에는 계속 사용할 수 있도록한다.
	//scope : application scope를 갖도록한다.
	public SqlSessionFactory getSqlSessionFactory() {
		init();//sqlSessionFactory의 객체 생성이 일어난다.
		return sqlSessionFactory;//init메서드를 거치지 않으면 null
	}
}
```

### Configuration.XML

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
		<mapper resource="oracle/mybatis/emp.xml"/>
		<mapper resource="oracle/mybatis/dept.xml"/>
	</mappers>
</configuration>
```

## Config.properties파일

```markup
#주석입니다.
driver = oracle.jdbc.driver.OracleDriver
url = jdbc:oracle:thin:@192.168.0.187:1521:orcl11
username = scott
password = tiger
```

