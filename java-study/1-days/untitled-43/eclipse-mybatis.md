# Eclipse : MyBatis, 클래스 조립

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

### 결과 : http://localHost:8080/board/boardInsert.sp3

![](../../../.gitbook/assets/1%20%2888%29.png)

![](../../../.gitbook/assets/2%20%2867%29.png)

