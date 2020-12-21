# Spring : 계층형 게시판

![](../../../.gitbook/assets/1%20%2893%29.png)

![](../../../.gitbook/assets/2%20%2871%29.png)

## JAVA

### 코드 : BoardController.java

```java
public class BoardController extends MultiActionController {//spring제공 클래스
	
	private BoardLogic boardLogic = null;

	public void setBoardLogic(BoardLogic boardLogic) {
		this.boardLogic = boardLogic;
	}

	public ModelAndView boardList(HttpServletRequest req, HttpServletResponse res)//viewResolver를 사용한다.
			throws Exception{
		logger.info("controller - boardList호출성공");
		List<Map<String,Object>> boardList = null;
		boardList = boardLogic.boardList(null);
		ModelAndView mav = new ModelAndView();
		mav.addObject("boardList", boardList);
		mav.setViewName("board/boardList");
		return mav;
	}
}
```

* pMap에 파라미터를 받아 조건값으로 넘겨줘야 하지만 일단 파라미터 없이 호출 테스트 하기 위해 null을 넘긴다.

### 코드 : BoardLogic.java

```java
public class BoardLogic {
	
	public SqlBoardMDao sqlBoardMDao = null;
	
	public void setSqlBoardMDao(SqlBoardMDao sqlBoardMDao) {
		this.sqlBoardMDao = sqlBoardMDao;
	}

	public SqlBoardDDao sqlBoardDDao = null;
	
	public void setSqlBoardDDao(SqlBoardDDao sqlBoardDDao) {
		this.sqlBoardDDao = sqlBoardDDao;
	}	

	public List<Map<String,Object>> boardList(Map<String, Object> pMap) {
		logger.info("Logic - boardList 호출 성공 : "+pMap);
		List<Map<String,Object>> bList = null;
		bList = sqlBoardMDao.boardList(pMap);
		return bList;
	}
}
```

