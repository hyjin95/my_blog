# XML

###  Oracle

```markup
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration
PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
 "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
	<environments default="development">
	<environment id="development">
	<transactionManager type="JDBC"/>
	<dataSource type="POOLED">
		<property name="driver" value="oracle.jdbc.driver.OracleDriver"/><!--드라이버 클래스 -->
		<property name="url" value="jdbc:oracle:thin:@192.168.123.7:1521:orcl11"/>
		<property name="username" value="scott"/><!--계정이름-->
		<property name="password" value="tiger"/><!--비밀번호-->
	</dataSource>
	</environment>
```

* DB제품 중 oracle을 이용한 설정
* 사용하려면 oracle드라이버 클래스가 있어야한다.

### mySQL

```markup
	<environment id="development2">
	<transactionManager type="JDBC"/>
	<dataSource type="POOLED">
		<property name="driver" value="com.mysql.jdbc.Driver"/>
		<property name="url" value="jdbc:mysql://127.0.0.1:3306/test"/>
		<property name="username" value="abc"/><
		<property name="password" value="123"/>
	</dataSource>
	</environment>
	
	</environments>
	<mappers>	
	</mappers>
</configuration>
```

* DB제품중 mySQL을 이용한 설정
* 사용하려면 mySQL드라이버 클래스가 있어야한다.

