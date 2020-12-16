# Eclipse : board - transaction, 클래스 조립

### 코드 : BoardController.java

```java
package com.example.demo;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.apache.logging.log4j.LogManager;
import org.apache.logging.log4j.Logger;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.servlet.ModelAndView;

@Controller
@RequestMapping("/board/*")
public class BoardController {
	Logger logger = LogManager.getLogger(BoardController.class);		
	
	//Autowired는 setter메서드를 사용하지 않더라도 객체주입을 받을 수 있게 해준다.
	@Autowired
	public BoardLogic boardLogic = null;
	
	@RequestMapping("/boardInsert.sp3")
	public String boardInsert(@RequestParam Map<String,Object> pMap) {
		logger.info("boardInsert 호출 성공 : "+pMap);
		int result = 0;
		result = boardLogic.boardInsert(pMap);
		return "redirect.boardInsertOk.jsp";
	}
}
```

### 코드 : BoardLogic.java

```java
package com.example.demo;

import org.apache.logging.log4j.LogManager;
import org.apache.logging.log4j.Logger;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

// AOP 프레임워크를 활용해 트랜잭션 처리하는 구간 : Logic

@Service
public class BoardLogic {
	Logger logger = LogManager.getLogger(BoardLogic.class);
	
	//false라면 해당 클래스가 없어도 에러가 발생하지 않는다. ture라면 필수조건이므로 에러가 발생한다.
	@Autowired(required=false)
	public SqlBoardMDao sqlBoardMDao = null;
	
	@Autowired(required=false)
	public SqlBoardDDao sqlBoardDDao = null;

	public int boardInsert(Map<String, Object> pMap) {//트랜잭션처리
		logger.info("Logic - boardInsert 호출 성공 : "+pMap);
		int result = 0;
		int mresult = sqlBoardMDao.boardMInsert(pMap);
		int dresult = sqlBoardDDao.boardDInsert(pMap);
		if(mresult == 1 && dresult ==1) {
			result =1;
		}
		return result;
	}
}
```

### 코드 : BoardMDao.java, BoardDDao.java

```java
package com.example.demo;

import java.util.Map;

import org.apache.logging.log4j.LogManager;
import org.apache.logging.log4j.Logger;
import org.springframework.stereotype.Service;

//Dao : Data Acces Object 디자인패턴 = Model계층, 업무와 관련이 있다.
@Service
public class SqlBoardMDao {
	Logger logger = LogManager.getLogger(SqlBoardMDao.class);

	public int boardMInsert(Map<String, Object> pMap) {//여기는 java : xml
		logger.info("boardM - boardInsert 호출 성공 : "+pMap);
		return 0;
	}
}
```

```java
package com.example.demo;

import java.util.Map;

import org.apache.logging.log4j.LogManager;
import org.apache.logging.log4j.Logger;
import org.springframework.stereotype.Service;

@Service
public class SqlBoardDDao {
	Logger logger = LogManager.getLogger(SqlBoardDDao.class);

	public int boardDInsert(Map<String, Object> pMap) {
		logger.info("boardD - boardInsert 호출 성공 : "+pMap);
		return 0;
	}
}
```

### 결과 : localHost:8080/board/boardInsert.sp3

![](../../../.gitbook/assets/dd%20%282%29.png)

