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

