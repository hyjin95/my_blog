# Spring : 게시판 - 새글, 댓글 작성

## JAVA

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
```

* import, 객체주입 부분

```java
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
```

* 새 게시글 작성이 성공적으로 처리되면 불려지는 메서드
* 사용자에게 갱신된 목록 화면을 제공한다.

```java
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

* 위 코드는 게시글만 insert하는 것 까지 구현되어 있어 첨부파일이 있는 경우 코드가 추가되어야 한다.
* 14번에서 새 게시글 작성 처리가 완료되면 갱신된 목록 화면을 사용자에게 제공한다.
* 16번에서 처리에 문제가 발생하면 안내 페이지를 사용자에게 제공한다.

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
```

* import, 객체주입 부분

```java
	public int boardInsert(Map<String, Object> pMap) {//트랜잭션처리
		logger.info("Logic - boardInsert 호출 성공 : "+pMap);
		int result = 0;
		int bm_no = 0;
		int bm_group =0;
```

* Insert메서드의 시작
* 필요한 변수들을 선언한다.
* insert결과를 담을 result, 게시글 번호를 담을 bm_no, 그룹번호를 담을 bm_group

```java
		if(pMap.get("bm_group")!=null) {
			bm_group = Integer.parseInt(pMap.get("bm_group").toString());
		}
		bm_no = sqlBoardMDao.getBmno();//pk이므로 마스터에서 가져온다.
		pMap.put("bm_no",bm_no);
```

* 게시글이라면 그룹번호를 갖고있지 않을 것이고, 댓글이라면 그룹번호를 갖고있게 된다.
* 파라미터에 그룹번호가 null이아니라면 댓글일 것이다.\
  파라미터에서 bm_group를 꺼내 담는다. \
  혹시모를 배달사고를 방지하기위해 toString으로 꺼내 형변환 한다.
* 글번호는 새 게시글 작성이던, 댓글작성이던 PK로서 SEQ를 순서대로 부여해야한다.\
  MDao의 메서드를 통해 다음에 부여될 글 번호를 가져와 pMap에 담는다.

```java
		//너 댓글이니?
		if(bm_group > 0) {
			pMap.put("bm_group",bm_group);
			sqlBoardMDao.bmStepUpdate(pMap);
			int pos = 0;
			int step = 0;
```

* bm_group변수가 >0 인 값을 담고 있다면 댓글업무이다.
* if문의 bm_group는 화면에서 가져온 것이므로 새 게시글이라면 0이겠지만 댓글의 경우는 read.jsp에서_ _가져온 bmgroup이 담겨있다. 해당 bm_group을 pMap에 담는다.
* 댓글은 끼워넣기를 해야하기때문에 같은 게시글에 대한 댓글이 여러개 존재한다면 작성된 댓글을 끼워넣기 위해 기존 댓글들의 순서에 +1을 해야한다. 3번코드에서 MDao의 메서드를 호출해 update한다.
* 이번에 작성된 댓글의 처리를 위해 글 차수와 순서를 담을 변수 두개를 선언한다.

```java
		//너 댓글이니??	
			if(pMap.get("bm_pos") != null) {//null체크 코드가 없을때 null이 발생하면 Exception이 발생할 수 있다.
				pos = Integer.parseInt(pMap.get("bm_pos").toString());
			}
			pMap.put("bm_pos", pos+1);
			
			if(pMap.get("bm_step") != null) {
				step = Integer.parseInt(pMap.get("bm_step").toString());
			}
			pMap.put("bm_step", step+1);
		}
```

* 파라미터에게서 null이 나오는 배달사고를 방지하기위해 null을 체크하는 if문을 작성한다.
* 변수 pos에 선택된 댓글을 달 게시글의 글 차수를 담는다.\
  Dao에게 파라미터로 넘길 map에 게시글 글 차수+1을 댓글의 글 차수로 담는다.\
  게시글보다 글 차수가 하나 높아야 하므로
* 변수 step에 선택된 댓글을 달 게시글의 글 순서를 담는다.\
  마찬가지로 해당 게시글보다 순서가 하나 높아야 하므로 +1해 map에 담는다.
* 이렇게 댓글인 경우의 pMap구성이 끝났다.

```java
		//새글이니?
		else {//pos, step이 0이다.
			bm_group = sqlBoardMDao.getBmGroup();//그룹번호는 채번하는 거라 파라미터는 필요없다.
			pMap.put("bm_pos", 0);
			pMap.put("bm_step", 0);
		}
```

* 댓글이 아닌 새 게시글 작성의 경우
* 새 게시글이므로 글 차수(pos)와 글 순선(step)는 0이여야 한다.
* 3번에서 MDao의 메서드를 호출해 새 게시글에게 부여할 게시글 번호를 채번해온다.
* 이렇게 새 게시글인 경우의 pMap구성이 끝났다.

```java
		int mresult = sqlBoardMDao.boardMInsert(pMap);
```

* 이제 새 게시물 작성이든 댓글 작성이든 Insert를 해야한다. 
* 만들어진 pMap을 파라미터로 보내 MDao의 Insert메서드를 호출한다.

```java
		//첨부파일이 있니?
		if(pMap.get("bs_file") != null && pMap.get("bs_file").toString().length() > 1) {
			pMap.put("bm_no", bm_no);
			int dresult = sqlBoardDDao.boardDInsert(pMap);
		}
		return mresult;
	}
}
```

* 첨부파일이 존재하는 경우는 따로 분류해 DDao에서 insert를 처리한다.
* 파라미터로 받아온 첨부파일 bm_file은 문자열일 것이기때문에 null체크와 같이 문자열 길이 체크도 해야한다.
* 4번에서 첨부파일 파라미터가 존재한다면 DDao의 insert메서드를 호출한다.

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
```

```java
	public List<Map<String, Object>> boardList(Map<String, Object> pMap) {
		logger.info("MDao - boardList 호출 성공 : "+pMap);
		List<Map<String,Object>> bList = null;
		bList = sqlSessionTemplate.selectList("boardList",pMap);
		return bList;
	}
```

```java
	public int getBmno() {
		int bm_no = 0;
		bm_no = sqlSessionTemplate.selectOne("getBmno");
		return bm_no;
	}
```

* Logic에서 새글 작성인 경우에 호출되는 메서드이다.
* 글 번호를 채번해 가져온다.

```java
	public void bmStepUpdate(Map<String, Object> pMap) {
		logger.info("boardM - bmStepUpdate 호출 성공 : "+pMap);
		int result = 0;
		result = sqlSessionTemplate.update("bmStepUpdate",pMap);
		logger.info(result);
	}
```

* 댓글 작성의 경우 Logic에서 호출되는 메서드이다.
* 댓글을 끼워넣기 위해 같은 게시글에 대한 댓글들의 bm_step에 +1한다.

```java
	public int getBmGroup() {
		logger.info("boardM -getBmGroup");
		int result = 0;
		result = sqlSessionTemplate.selectOne("getBmGroup");
		logger.info(result);
		return result;
	}
}
```

* 새 게시글 작성의 경우 Logic에서 호출되는 메서드이다.
* 새 게시글에게 부여할 그룹번호를 채번한다.

```java
	public int boardMInsert(Map<String, Object> pMap) {//여기는 java : xml
		//insert into board_master_t(b_no, b_title, .....) values(seq_board_no.nextval, "안녕하세요", .....)
		logger.info("boardM - boardInsert 호출 성공 : "+pMap);
		int result = 0;
		result = sqlSessionTemplate.insert("boardMInsert",pMap);
		logger.info(result);
		return result;
	}
```

* 새 게시글 작성 또는 댓글 작성에 대한 정보가 담긴 pMap을 받아 insert 쿼리문을 호출한다.
* insert이기 때문에 결과 값은 0또는 1이 나온다.

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

* 첨부파일을 담당하는 Dao로서 첨부파일이 존재할때에만 불려지는 메서드들을 갖는다.

## myBatis.xml

### 쿼리문 : board.xml

```markup
  <select id="getBmno" parameterType="int" resultType="int">
		SELECT seq_board_no.nextval bm_no FROM dual
	</select>
```

* Dao에서 새 글 작성시 호출되는 쿼리문이다.
* 시퀀스로 만들어둔 bm_no(PK)의 다음값을 불러온다. = 새 게시글의 번호 채번

```markup
	<update id="bmStepUpdate" parameterType="map">
		UPDATE board_master_t
		   SET bm_step = bm_step + 1
		 WHERE bm_group = #{bm_group} 
		   AND bm_step > #{bm_step};
	</update>
```

* 댓글 작성은 게시글 상세보기 페이지에서 가능하기 때문에 선택된 하나의 게시글이 존재한다.\
  select된 한개 row를 갖고있다.
* 해당 게시글의 그룹번호와 일치하고 게시글보다 순서가 높은 모든 댓글들의 차수에 +1해 update한다.
* 해당 게시글의 1번 bm_step은 새 댓글이다.

```markup
	<select id="getBmGroup" parameterType="map" resultType="map">
		SELECT
   			  NVL((SELECT /*+index_desc(board_master_t iboard_group)*/ bm_group
         			 FROM board_master_t
        			WHERE bm_group > 0
         			  AND rownum = 1),0) +1 
 		 FROM dual;
	</select>
```

* 새 게시글 작성의 경우 새로운 그룹번호를 가져야한다. 
* 미리 index를 부여해놓은 그룹번호 컬럼을 통해 빠른 검색을 한다.
* 가장 마지막에 부여된 그룹번호에 +1을 해서 채번한다.
* 작성된 새 게시글이 첫 게시글이라면 해당 테이블 조회 값이 null이 나오고, 에러가 발생할 것이기 때문에 NVL구문을 작성해 이를 방지하고, null이라면 1을 가질수 있도록 했다.

```markup
	<select id="boardList" parameterType="map" resultType="map"><!-- bs테이블은 null일 수 있으므로 nullpointer방지로 NVL구문 사용 -->		   
		SELECT bm.bm_no, bm.bm_title, bm.bm_writer, bm.bm_content, bm.bm_email,bm.bm_pw, bm.bm_group, bm.bm_pos, bm.bm_step, bm.bm_hit, bm.bm_date
			   ,NVL(bs.bs_file,'') bs_file, NVL(bs.bs_size,0) bs_size 
		 FROM board_master_t bm left outer join board_sub_t bs
		   ON bm.bm_no = bs.bm_no
		 <where>
		 	<if test="bm_no > 0">
		 		AND bm.bm_no=#{bm_no}
		 	</if>
		 </where>
		 ORDER BY bm_no desc
	</select>
```

```markup
	<select id="seqBoardNo" parameterType="map" resultType="map">
		SELECT SEQ_BOARD_NO.nextval FROM dual
	</select>
```

```markup
	<insert id="boardMInsert" parameterType="map"><!-- 끝에 세미콜론 들어가면 안 -->
		INSERT INTO board_master_t(bm_no,bm_writer,bm_title,bm_content,bm_email,bm_date,bm_group,bm_pos,bm_step,bm_pw)
       	 VALUES(5,'작성자5','제목5','내용5','test5@hot.com','2020-12-22',0,0,0,'123')
	</insert>	  
	
	<insert id="boardDInsert" parameterType="map"><!-- 끝에 세미콜론 들어가면 안됨 -->	   
		INSERT INTO board_sub_t(bm_no, bs_seq, bs_file, bs_size)
		   VALUES(5,1,'test5.txt',10)
	</insert>	  
```

* Insert 쿼리문
* 현재는 상수로 박아 놨지만 추후에 파라미터로 받은 pMap으로 대체 해야한다.
* \#{값}\
  값 = 파라미터로 받아온 map의 키 이름이 들어간다.
