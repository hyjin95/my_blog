# Eclipse : MyBatis, 클래스 조립, application.properties

## 프로젝트 경로

![](../../../.gitbook/assets/.png%20%2849%29.png)

## 연결

### 코드 : application.properties

```elixir
server.port=8080
#request에 대한 응답페이지 설정 추가
spring.mvc.view.prefix=/WEB-INF/views/
spring.mvc.view.suffix=.jsp
#톰캣서버와 스프링 프로토콜 요청시 한글 설정 추가
server.tomcat.uri-encoding=UTF-8
#spring.http.encoding.charset=UTF-8 내장되어있나?
#HikariCP 커넥션 풀 설정 추가
spring.datasource.hikari.driver-class-name=oracle.jdbc.driver.OracleDriver
spring.datasource.hikari.jdbcUrl=jdbc:oracle:thin:@127.0.0.1:1521:orcl11
spring.datasource.hikari.username=scott
spring.datasource.hikari.password=tiger
spring.datasource.hikari.connection-timeout=20000
spring.datasource.hikari.minimum-Idle=5
spring.datasource.hikari.maximum-pool-size=12
spring.datasource.hikari.ldle-timeout=300000
spring.datasource.hikari.max-lifetime=1200000
spring.datasource.hikari.auto-commit=true
```

### 코드 : DatabaseConfiguration.java

```java
package com.example.demo;

import javax.sql.DataSource;

import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.logging.log4j.LogManager;
import org.apache.logging.log4j.Logger;
import org.mybatis.spring.SqlSessionFactoryBean;
import org.mybatis.spring.SqlSessionTemplate;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.PropertySource;

import com.zaxxer.hikari.HikariConfig;
import com.zaxxer.hikari.HikariDataSource;

//spring-data.xml을 java로 구현했다.
//spring-context.jar가 있기에 가능하다. -> ApplicationContext로 빈관리를 받아 소멸과 생성을 담당해준다.

@Configuration//환경설정 xml : <context-param><param-name>contextConfigLocation<param-value>/WEB-INF/spring-data.xml
//MapperScan(basePackages = {"com.example.demo"})
@PropertySource("classpath:/application.properties")// 3 : 여기에 정보있다. 29번은 여기에 접근하기 위한 prefix이다.
public class DatabaseConfiguration {//hikariCP라는 오브젝트를 사용하기
   private static final Logger logger = LogManager.getLogger(DatabaseConfiguration.class);
   @Bean//다 bean으로 묶어놧기때문에 spring-context의 클래스가 bean으로 관리를 해줄 수 있는 것이다.
   @ConfigurationProperties(prefix = "spring.datasource.hikari")// 2 : 히카리컨피그 객체 생성, 오라클 서버에 대한 정보를 갖고있음, 어디에잇다> spring.datasource,hikari에있다.
   public HikariConfig hikariConfig() {
      return new HikariConfig();//오라클에대한 서버 정보
   }

   @Bean
   public DataSource dataSource() {// 1 : Connection con = DriverManager.getConnection(url, scott, tiger,...);
      DataSource dataSource = new HikariDataSource(hikariConfig());//생성자에 함수 호출 
      logger.info("datasource : {}", dataSource);
      return dataSource;//연결통로에 대한 정보를 갖게됨, 주소번지 -> 이걸로 sqlSessionFactory Bean을 만들어야함
   }
   @Autowired
   private ApplicationContext applicationContext;

   @Bean
   public SqlSessionFactory sqlSessionFactory(DataSource dataSource) throws Exception {//datasource로 연결통로 생성 = connection
      SqlSessionFactoryBean sqlSessionFactoryBean = new SqlSessionFactoryBean();
      sqlSessionFactoryBean.setDataSource(dataSource);
      //classpath는 src/main/resourcs이고 해당 쿼리가 있는 xml 위치는 본인의 취향대로 위치키시고 그에 맞도록 설정해주면 된다.
      sqlSessionFactoryBean.setMapperLocations(applicationContext.getResources("classpath:/mapper/**/*.xml"));//쿼리문(mybatis문서)에대한 물리적 위치
      return sqlSessionFactoryBean.getObject();
   }
   
   /* <bean id="sqlSessionTemplate" class="xxx.xxx.sqlSessionTemplate>
    * 	<contsructor-arg index=0 ref="sqlSqssionFactory"/>
    * </bean>
    */

   @Bean
   public SqlSessionTemplate sqlSessionTemplate(SqlSessionFactory sqlSessionFactory) {
      return new SqlSessionTemplate(sqlSessionFactory);
   }   
}
```

## Dao + xml + jsp 코드

### 코드 : BoardMDao.java

```java
package com.example.demo;

import java.util.Map;

import org.apache.logging.log4j.LogManager;
import org.apache.logging.log4j.Logger;
import org.mybatis.spring.SqlSessionTemplate;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

//Service또는 Repository 어노테이션 역할을 똑같다.
//Dao : Data Acces Object 디자인패턴 = Model계층, 업무와 관련이 있다.
@Service
public class SqlBoardMDao {
	Logger logger = LogManager.getLogger(SqlBoardMDao.class);
	
	@Autowired
	private SqlSessionTemplate sqlSessionTemplate = null;

	public int boardMInsert(Map<String, Object> pMap) {//여기는 java : xml
		//insert into board_master_t(b_no, b_title, .....) values(seq_board_no.nextval, "안녕하세요", .....)
		logger.info("boardM - boardInsert 호출 성공 : "+pMap);
		int result = 0;
		result = sqlSession.insert("boardInsert",pMap);
		return result;
	}
}
```

### 코드 : BoardDDao.java

```java
package com.example.demo;

import java.util.Map;

import org.apache.logging.log4j.LogManager;
import org.apache.logging.log4j.Logger;
import org.mybatis.spring.SqlSessionTemplate;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class SqlBoardDDao {
	Logger logger = LogManager.getLogger(SqlBoardDDao.class);
	
	@Autowired
	private SqlSessionTemplate sqlSessionTemplate = null;

	public int boardDInsert(Map<String, Object> pMap) {
		logger.info("boardD - boardInsert 호출 성공 : "+pMap);
		int result = 0;
		result = sqlSession.insert("boardInsert",pMap);
		return result;
	}
}
```

### 코드 : boardInsert.jsp

```markup
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<script>
	alert("정상처리 완료");
	console.log("등록완료");
</script>
```

### 코드 : board.xml

```markup
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.example.demo"><!-- 패키지 맞추기 -->

 <select id="cday" parameterType="map" resultType="int">
	insert into board_master_t(b_no, b_title, .....) values(seq_board_no.nextval, "안녕하세요", .....)
 </select>
 
</mapper>
```

## 결과 : url

### http://localHost:8080/board/boardInsert.sp3

![alert](../../../.gitbook/assets/1%20%2888%29.png)

![console.log](../../../.gitbook/assets/2%20%2868%29.png)

