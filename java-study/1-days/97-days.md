---
description: 2021.01.05 - 97일차
---

# 97 Days -

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org, Toad DBA Suite for Oracle 11.5 , Spring, Android Studio
* 사용 서버 - WAS : Tomcat

## Spring-boot  : 조회수

### 게시판 : boardList.jsp

```javascript
<a href="javascript:boardDetail('<%=rmap.get("BM_NO")%>')">
    <%=rmap.get("BM_TITLE") %>
</a>
```

* 제목을 클릭하면 상세보기페이지로 이동하고, 이때 조회수가 카운트 되어야 한다.

### Controller

```java
@RequestMapping("/boardDetail.sp3")
	public String boardDetail(Model mod, @RequestParam Map<String,Object> pMap) {
		logger.info("controller - boardDetail호출성공 : "+ pMap.get("gubun"));
		List<Map<String,Object>> boardList = null;
		pMap.put("gubun","detail");
		boardList = boardLogic.boardList(pMap);
		mod.addAttribute("boardDetail", boardList);
		return "board/read";
	}
```

* Controller에서 메서드를 추가할 필요없이 상세보기 페이지로 넘어갈때 조회수가 올라가기만 하면된다.

### Logic

```java
public List<Map<String,Object>> boardList(Map<String, Object> pMap) {
		logger.info("Logic - boardList 호출 성공 : "+pMap);
		List<Map<String,Object>> bList = null;
		bList = sqlBoardMDao.boardMList(pMap);
		String gubun ="";
		if(pMap.get("gubun")!=null) {
			gubun = pMap.get("gubun").toString();
		}
		//조건을 두가지로 한다. : 조건검색의 결과가 하나의 로우인 경우도 포함되므로 gubun키값을 추가했따.
		if(bList.size()==1 && "detail".equals(gubun)) {
			int bm_no = 0;
			//null체크
			if(pMap.get("bm_no")!=null) {
				bm_no = Integer.parseInt(pMap.get("bm_no").toString());
			}
			sqlBoardMDao.hitCount(bm_no);
		}
		return bList;
	}
```

* Controller에서 boardList\(전체조회\)업무도, boardDetail\(상세보기\)업무도 Logic의 같은 boardList를 활용하고 있기 때문에 두 업무를 

### SQL

```markup
	<update id="hitCount" parameterType="int">
		UPDATE board_master_t
		   SET bm_hit = bm_hit +1
		 WHERE bm_no = #{value}
	</update>
```

* board.xml에 쿼리문 작성

## Spring-boot : 삭제

## Android Studio 

### StopWatchAfter.apk

* 현재상태를 저장하기 위해 메서드를 오버라이드 한다. - onSaveInstanceState\(Bundle savedInstanceState\) - 메서드 안에서 반드시 부모\(상위\)메서드를 불러와야 한다.
* 저장해야 되는 정보 - 시간
* Life Cycle을 고려한 APK

