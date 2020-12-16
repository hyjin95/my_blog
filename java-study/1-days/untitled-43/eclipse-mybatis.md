# Eclipse : MyBatis, 클래스 조립, application.properties

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
		//result = sqlSession.insert("boardInsert",pMap);
		String cday = sqlSessionTemplate.selectOne("cday",pMap);
		logger.info(cday);
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
		//result = sqlSession.insert("boardInsert",pMap);
		String cday = sqlSessionTemplate.selectOne("cday",pMap);
		logger.info(cday);
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

 <select id="cday" parameterType="int" resultType="string">
	SELECT TO_CHAR(sysdate,'YYYY-MM-DD') cday
	  FROM dual
 </select>
 
</mapper>
```

## 

### application.properties

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

## 결과 : url

### http://localHost:8080/board/boardInsert.sp3

![](../../../.gitbook/assets/1%20%2888%29.png)

![](../../../.gitbook/assets/2%20%2867%29.png)

