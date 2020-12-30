---
description: 2020.12.30 - 94일차
---

# 94 Days -

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org, Toad DBA Suite for Oracle 11.5 , Spring, Android Studio
* 사용 서버 - WAS : Tomcat

## Spring : 첨부파일

### Spring 환경설정 방법

1. xml기반 환경설정
2. xml대신 application.properties 설정
3. java로 환경 설정

### Spring : Controller

```java
//첨부파일은 필수조건이 아니므로 nullPointer를 피하려면 required를 붙여줘야 한다.
	public void boardInsert(HttpServletRequest req, HttpServletResponse res)
			throws Exception{
		logger.info("controller - boardInsert호출성공");
		int result = 0;
		Map<String,Object> pmap = new HashMap<>();
		HashMapBinder hmb = new HashMapBinder(req);
		//첨부파일을 처리할 때에는 반드시 post방식이여야 한다.
		//주의사항 : request로는 사용자가 입력한 값을 받을 수가 없다.
		//따라서 cos.jar에서 제공하는 MultipartRequest를 사용해야만 값을 요청할 수 있다.
		hmb.multiBind(pmap);
		result = boardLogic.boardInsert(pmap);//첨부파일 여부는 logic에서 처리한다.
		if(result==1) {
			res.sendRedirect("/board/boardInsertOk.jsp");//insert성공 후 목록 갱신, 보여주기
		}else {
			res.sendRedirect("/board/boardInsertFail.sp");
		}
	}
```

### Spring-boot : Controller

```java
//@RequestMapping("/boardInsert.sp3")
	@PostMapping("/boardInsert.sp3")//RequestParam = HashMapBinder역할, 첨부파일은 어떻게 받지?? RequestParam을 하나 더 설정한다. 어노테이션뒤에 ( )안에 속성을 설정할 수 있다.
	public String boardInsert(@RequestParam Map<String,Object> pMap
							, @RequestParam(value="bs_file", required=false) MultipartFile bs_file) {//required=false, 첨부파일은 없을 수 있다.
		logger.info("controller - boardInsert호출성공");
		int result = 0;
		//첨부파일을 처리할 때에는 반드시 post방식이여야 한다.
		//주의사항 : request로는 사용자가 입력한 값을 받을 수가 없다.
		//따라서 cos.jar에서 제공하는 MultipartRequest를 사용해야만 값을 요청할 수 있다.
		result = boardLogic.boardInsert(pmap);//첨부파일 여부는 logic에서 처리한다.
		if(result==1) {
			return "redirect:boardInsertOk.jsp";
		}else {
			return "redirect:boardInsertFail.sp";//Insert이기때문에 forward일 필요 없다.
		}
	}
```

