# Spring : 게시판 - 새글, 댓글 작성

### 코드 : BoardController.java

```java
package com.board;

import java.util.HashMap;
import java.util.List;
import java.util.Map;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.apache.log4j.Logger;
import org.springframework.web.servlet.ModelAndView;
import org.springframework.web.servlet.mvc.multiaction.MultiActionController;

import com.util.HashMapBinder;

public class BoardController extends MultiActionController {//spring제공 클래스
	Logger logger = Logger.getLogger(BoardController.class);
	
	private BoardLogic boardLogic = null;

	public void setBoardLogic(BoardLogic boardLogic) {
		this.boardLogic = boardLogic;
	}

	//전체조회나 조건검색 구현
	public ModelAndView boardList(HttpServletRequest req, HttpServletResponse res)//viewResolver를 사용한다.
			throws Exception{
		logger.info("controller - boardList호출성공");
		List<Map<String,Object>> boardList = null;
		//조건검색시
		Map<String,Object> pmap = new HashMap<>();
		HashMapBinder hmb = new HashMapBinder(req);
		hmb.bind(pmap);
		boardList = boardLogic.boardList(pmap);
		ModelAndView mav = new ModelAndView();
		mav.addObject("boardList", boardList);
		mav.setViewName("board/boardList");
		return mav;
	}
	
	public void boardInsert(HttpServletRequest req, HttpServletResponse res)
			throws Exception{
		logger.info("controller - boardInsert호출성공");
		int result = 0;
		Map<String,Object> pmap = new HashMap<>();
		HashMapBinder hmb = new HashMapBinder(req);
		hmb.bind(pmap);
		//첨부파일이 존재하니?
		if("첨부파일".equals("첨부파일")) {
			
		}
		result = boardLogic.boardInsert(pmap);
		if(result==1) {
			res.sendRedirect("board/boardList.sp");//insert성공 후 목록 갱신, 보여주기
		}else {
			res.sendRedirect("board/boardInsertFail.sp");
		}
	}
}
```

### 코드 : BoardLogic.java

```java
package com.board;

import java.util.List;
import java.util.Map;

import org.apache.log4j.Logger;

/*
 * AOP 프레임워크를 활용해 트랜잭션 처리하는 구간 : Logic
 */
public class BoardLogic {
	Logger logger = Logger.getLogger(BoardLogic.class);
	
	public SqlBoardMDao sqlBoardMDao = null;
	
	public void setSqlBoardMDao(SqlBoardMDao sqlBoardMDao) {
		this.sqlBoardMDao = sqlBoardMDao;
	}

	public SqlBoardDDao sqlBoardDDao = null;
	
	public void setSqlBoardDDao(SqlBoardDDao sqlBoardDDao) {
		this.sqlBoardDDao = sqlBoardDDao;
	}	

	public int boardInsert(Map<String, Object> pMap) {//트랜잭션처리
		logger.info("Logic - boardInsert 호출 성공 : "+pMap);
		int result = 0;
		int bm_no = 0;
		int bm_group =0;
		if(pMap.get("bm_group")!=null) {
			bm_group = Integer.parseInt(pMap.get("bm_group").toString());
		}
		bm_no = sqlBoardMDao.getBmno();//pk이므로 마스터에서 가져온다.
		//너 댓글이니?
		if(bm_group > 0) {
			sqlBoardMDao.bmStepUpdate(pMap);
			int pos = 0;
			int step = 0;
			
			if(pMap.get("bm_pos") != null) {//null체크 코드가 없을때 null이 발생하면 Exception이 발생할 수 있다.
				pos = Integer.parseInt(pMap.get("bm_pos").toString());
			}
			pMap.put("bm_pos", pos+1);
			
			if(pMap.get("bm_step") != null) {//null체크 코드가 없을때 null이 발생하면 Exception이 발생할 수 있다.
				step = Integer.parseInt(pMap.get("bm_step").toString());
			}
			pMap.put("bm_step", step+1);
		}
		//새글이니?
		else {//pos, step이 0이다.
			bm_group = sqlBoardMDao.getBmGroup();//그룹번호는 채번하는 거라 파라미터는 필요없다.
			pMap.put("bm_pos", 0);
			pMap.put("bm_step", 0);
		}
		int mresult = sqlBoardMDao.boardMInsert(pMap);
		//첨부파일이 있니?
		if(pMap.get("bs_file") != null && pMap.get("bs_file").toString().length() > 1) {
			pMap.put("bm_no", bm_no);
			int dresult = sqlBoardDDao.boardDInsert(pMap);
		}
		return result;
	}
}
```

### 코드 : BoardMDao.java

```java
package com.board;

import java.util.List;
import java.util.Map;

import org.apache.log4j.Logger;
import org.mybatis.spring.SqlSessionTemplate;

public class SqlBoardMDao {
	Logger logger = Logger.getLogger(SqlBoardMDao.class);
	
	private SqlSessionTemplate sqlSessionTemplate = null;

	public void setSqlSessionTemplate(SqlSessionTemplate sqlSessionTemplate) {
		this.sqlSessionTemplate = sqlSessionTemplate;
	}

	public int boardMInsert(Map<String, Object> pMap) {//여기는 java : xml
		//insert into board_master_t(b_no, b_title, .....) values(seq_board_no.nextval, "안녕하세요", .....)
		logger.info("boardM - boardInsert 호출 성공 : "+pMap);
		int result = 0;
		result = sqlSessionTemplate.insert("boardMInsert",pMap);
		logger.info(result);
		return result;
	}

	public List<Map<String, Object>> boardList(Map<String, Object> pMap) {
		logger.info("MDao - boardList 호출 성공 : "+pMap);
		List<Map<String,Object>> bList = null;
		bList = sqlSessionTemplate.selectList("boardList",pMap);
		return bList;
	}

	public int getBmno() {
		int bm_no = 0;
		bm_no = sqlSessionTemplate.selectOne("getBmno");
		return bm_no;
	}

	public void bmStepUpdate(Map<String, Object> pMap) {
		logger.info("boardM - bmStepUpdate 호출 성공 : "+pMap);
		int result = 0;
		result = sqlSessionTemplate.update("bmStepUpdate",pMap);
		logger.info(result);
	}

	public int getBmGroup() {
		logger.info("boardM -getBmGroup");
		int result = 0;
		result = sqlSessionTemplate.update("getBmGroup");
		logger.info(result);
		return result;
	}
}
```

### 코드 : BoardDDao.java

```java
package com.board;

import java.util.Map;

import org.apache.log4j.Logger;
import org.mybatis.spring.SqlSessionTemplate;


public class SqlBoardDDao {
	Logger logger = Logger.getLogger(SqlBoardDDao.class);
	
	private SqlSessionTemplate sqlSessionTemplate = null;

	public void setSqlSessionTemplate(SqlSessionTemplate sqlSessionTemplate) {
		this.sqlSessionTemplate = sqlSessionTemplate;
	}

	public int boardDInsert(Map<String, Object> pMap) {
		logger.info("boardD - boardInsert 호출 성공 : "+pMap);
		int result = 0;
		result = sqlSessionTemplate.insert("boardDInsert",pMap);
		logger.info(result);
		return result;
	}
}

```

