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
	public void boardInsert(HttpServletRequest req, HttpServletResponse res)
			throws Exception{
		logger.info("controller - boardInsert호출성공");
		int result = 0;
		Map<String,Object> pmap = new HashMap<>();
		HashMapBinder hmb = new HashMapBinder(req);
		//첨부파일을 처리할 때에는 반드시 post방식이여야 한다.
		//주의사항 : cos.jar에서 제공하는 MultipartRequest를 사용해야 값 요청 가능
		hmb.multiBind(pmap);
		result = boardLogic.boardInsert(pmap);//첨부파일 여부는 logic에서 처리한다.
		if(result==1) {
			res.sendRedirect("/board/boardInsertOk.jsp");//insert성공 후 목록 갱신
		}else {
			res.sendRedirect("/board/boardInsertFail.sp");
		}
	}
```

* HttpServlet을 주입받아 사용한다.
* POJO로 작성한 HashMapBinder클래스를 생성, 요청객체를 넘겨 사용자 입력값, 첨부파일을 받아온다.
* 기존 request객체로는 파일타입을 처리할 수 없어 cos.jar를 추가해 MultipartRequest객체를 사용한다.
* 응답도 HttpServlet을 주입받아 response객체를 사용해 응답한다.

### Spring-boot : Controller

```java
  //첨부파일은 어떻게 받지?? RequestParam을 하나 더 설정한다. 어노테이션뒤에 ( )안에 속성을 설정할 수 있다.
  //@RequestMapping("/boardInsert.sp3")
	@PostMapping("/boardInsert.sp3")
	public String boardInsert(@RequestParam Map<String,Object> pMap
							, @RequestParam(value="bs_file", required=false) MultipartFile bs_file) {//required=false, 첨부파일은 없을 수 있다.
		
		logger.info("controller - boardInsert호출성공");
		int result = 0;
		result = boardLogic.boardInsert(pmap);//첨부파일 여부는 logic에서 처리한다.
		if(result==1) {
			return "redirect:boardInsertOk.jsp";
		}else {
			return "redirect:boardInsertFail.sp";//Insert이기때문에 forward일 필요 없다.
		}
	}
```

* 첨부파일이 포함될 수 있으므로 Post방식으로 요청이 들어올것이므로 @PostMapping어노테이션을 사용했다.
* RequestParam 1 : 사용자 입력값을 받아올 파라미터 Map, HashMapBinder클래스 역할을 수행한다.
* RequestParam 2 : 첨부파일을 받아올 파라미터 MultipartFile - 첨부파일은 필수조건이 아니기 떄문에 required=falsle속성을 추가해 없더라도 지나가게 처리한다.
* 타입이 String이므로 return이 필요하고 응답처리는 response객체를 주입받아 사용하지 않는다.

