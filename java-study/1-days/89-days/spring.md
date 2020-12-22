# Spring : 게시판 - 상세보기

### 코드 : BoardController.java

```java
//상세보기 페이지 구현
	public ModelAndView boardDetail(HttpServletRequest req, HttpServletResponse res)//viewResolver를 사용한다.
			throws Exception{
		logger.info("controller - boardDetail호출성공");
		List<Map<String,Object>> boardList = null;
		Map<String,Object> pmap = new HashMap<>();
		HashMapBinder hmb = new HashMapBinder(req);
		hmb.bind(pmap);
		boardList = boardLogic.boardList(pmap);
		ModelAndView mav = new ModelAndView();
		mav.addObject("boardDetail", boardList);
		mav.setViewName("board/read");
		return mav;
	}
```

### 코드 : BoardLogic.java

```java
	public List<Map<String,Object>> boardList(Map<String, Object> pMap) {
		logger.info("Logic - boardList 호출 성공 : "+pMap);
		List<Map<String,Object>> bList = null;
		bList = sqlBoardMDao.boardList(pMap);
		return bList;
	}
```

* 기존의 전체 조회와 같은 메서드를 활용한다.

### 코드 : BoardMDao.java

```java
	public List<Map<String, Object>> boardList(Map<String, Object> pMap) {
		logger.info("MDao - boardList 호출 성공 : "+pMap);
		List<Map<String,Object>> bList = null;
		bList = sqlSessionTemplate.selectList("boardList",pMap);
		return bList;
	}
```

* 기존의 전체 조회와 같은 메서드, 같은 SQL문을 사용한다.

### xml : Board.xml

```markup
	<select id="boardList" parameterType="map" resultType="map"><!-- bs테이블은 null일 수 있으므로 nullpointer방지로 NVL구문 사용 -->		   
		SELECT 
			   bm.bm_no, bm.bm_title, bm.bm_writer, bm.bm_content, bm.bm_email
			   ,bm.bm_pw, bm.bm_group, bm.bm_pos, bm.bm_step, bm.bm_hit, bm.bm_date
			   ,NVL(bs.bs_file,'') bs_file, NVL(bs.bs_size,0) bs_size
		  FROM board_master_t bm right outer join board_sub_t bs
		    ON bm.bm_no = bs.bm_no
		 <where>
		 	<if test="bm_no > 0">
		 		AND bm.bm_no=#{bm_no}
		 	</if>
		 </where>
		 ORDER BY bm_no desc
	</select>
```

