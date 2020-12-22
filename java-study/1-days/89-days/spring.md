# Spring : 게시판 - 상세보기

## JAVA

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

* 만들어 두었던 HashMapBinder클래스를 활용해 사용자의 입력값을 받아온다.
* 기존의 전체 조회와 같은 메서드와 sql문을 활용한다.

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

## XML

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

* 8-12번 전체조회면 파라미터로 받아오는 조건이 없기때문에 where절이 무시되고, 상세조회는 bm\_no 를 파라미터로 받아오기 때문에 where절이 유효하다.

## HashMapBinder

### 코드 : HashMapBinder.java

```java
package com.util;

import java.util.Enumeration;
import java.util.Map;
import javax.servlet.http.HttpServletRequest;
import jdk.nashorn.internal.ir.RuntimeNode.Request;

/*
 * 공통코드
 * 사용자로부터 입력받는 값을 효과적으로 처리
 */

public class HashMapBinder {
	public HttpServletRequest request = null;
	
	public HashMapBinder(HttpServletRequest request) {
		this.request = request;//원본을 사용해야한다.	서블릿에서 인스턴스화	
	}
	
	public void bind(Map<String, Object> target) {
		target.clear();
		Enumeration<String> en = request.getParameterNames();//getParameter로 받아오는 값의 리턴타입은 String이므로 Enumeration의 타입도 String으로 한다.
		while(en.hasMoreElements()) {//hasMoreElement는 boolean타입 메서드
			//<input name="meme_id"
			String key = en.nextElement();//mem_id, mem_pw, mem_addr,.....
			target.put(key, HangulConversion.toUTF(request.getParameter(key)));//한글 인코딩, post방식은 따로 해줘야댐
		}
	}
```

### 코드 : HangulConversion.java

```java
package com.util;
//한글 처리
public class HangulConversion {
		
	public static String toKor(String en) {
		if(en == null) return null;
		try {
			return new String(en.getBytes("8859_1"),"euc-kr");
		} catch (Exception e) {
			return en;
		}
	}////end of toKor
	
	public static String toUTF(String en) {
		if(en == null) return null;
		try {
			return new String(en.getBytes("8859_1"),"utf-8");
		} catch (Exception e) {
			return en;
		}
	}////end of toKor
}
```

## 결과 : url

### /board/boardDetail.sp?bm\_no=2

![](../../../.gitbook/assets/1%20%2896%29.png)

