---
description: 2020.01.04 - 96일차
---

# 96 Days -

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org, Toad DBA Suite for Oracle 11.5 , Spring, Android Studio
* 사용 서버 - WAS : Tomcat

## Spring

### 웹과 앱

* 웹 : html
* 앱 : apk

### 화면 페이지 배포 위치

* webapp &gt; board
* WEB-INF &gt; views &gt; board : url로 접근이 불가능한 배포위치
* 페이지 이동을 호출하는 구간은 Controller구간이다.
* 메서드의 파라미터 타입의 리턴 값에 따라 달라진다.
* **void** - req, res에 의존적 - webapp &gt; board
* **ModelAndView** - req, res에 의존적 - WEB-INF &gt; views &gt; board
* **String**  - req, res가 없어도 된다. - webapp &gt; board  **** return "redirect:xxx.jsp \| xxx.do"   return "forward:xxx.jsp" - forward는 서블릿 요청이 불가 - WEB-INF &gt; views &gt; board   return "board/boardList"와 같이 하면 ModelAndView와 동일한 위치

### UI와 DataSet

* JS기반  - JSON으로 처리한다.    json으로 만들어진 jsonBoardList.jsp페이지가 필요하지만,    UI에서 직접 값을 꺼내 담는 자바 코드는 필요없다. - 부트스트랩, 시멘틱UI, easyUI, 안드로이드 - 특별한 경우를 제외하면 오라클과 연동하지 않으므로 noSQL제품과 연동하는 경우가 많다.
* XMl기반 - 조회결과가 xml문서\(text/xml\)로 내보내져 xmlBoardList.jsp페이지가 필요하다. - 넥사크로, 플렉스, 트러스트폼, .... - 넥사크로에서 조회된 결과는 넥사크로에서 제공하는 DataSet객체에 담아야 한다.   화면을 지원하는 Grid와 DataSet을 매핑해 UI에 출력, 완성한다. - 넥사크로에서는 xmlBoardList.jsp를 만드는 대신 직접 자바단에서 xml포맷을 생성해주는 API를 지원한다.

### Spring-boot 첨부파일 코드분석 : Controller

```java
//@RequestMapping("/boardInsert.sp3")
	@PostMapping("/boardInsert.sp3")//RequestParam = HashMapBinder역할, 첨부파일은 어떻게 받지?? RequestParam을 하나 더 설정한다. 어노테이션뒤에 ( )안에 속성을 설정할 수 있다.
	public String boardInsert(@RequestParam Map<String,Object> pMap
							, @RequestParam(value="bs_file", required=false) MultipartFile bs_file) {//required=false, 첨부파일은 없을 수 있다.
		logger.info("controller - boardInsert호출성공");
		int result = 0;
		
		String path = null;
		String pathSave= "C:\\workspace_java\\demo\\src\\main\\webapp\\pds";
		String filename = null;
		String fullPath= null;
		//프레임워크가 반복되는 코드를 줄여 줄 수 있다.
		//예) 사용자가 입력한 값 전달하기(req.getParameter(여기만 다름),반복되는 코드 -> @RequestParam)
		//예) select의 경우 유지처리하기(jsp와  java사이의 forward(페이지이동)
		//	 Spring 제공클래스 Model, ModelMap지원 : ui를 지원하기위한 api
		//	 POJO : req.setAttribute("이름",값);
		//	 자료구조 = 컬렉션프레임워크 = 파이썬(리스트(자바 list), 튜플(상수 리스트:순서는 있지만 변경불가), 딕셔너리(자바 map), 집합{자바 set) : 자료의 도식화
		//	 json(web,안드로이드0, 넥사크로(xml그리고 js변환해줌) - DataSet지원, Post방식이 기본
		//파라미터의 값이 null일경우를 고려하면 required옵션을 false로 두어야 한다. not null이 아닌 값인 경우
		if(bs_file != null) {//첨부파일이 있다면
			filename = bs_file.getOriginalFilename();//본래의 파일 명을 가져온다.
			fullPath = pathSave+"\\"+filename;//물리적 경로와 파일 명을 연결해 위치정보를 다시 저장한다.
		}
		
		//첨부파일 이름은?
		if(bs_file != null && bs_file.isEmpty()) {
			try {
				//파일 명을 객체로 만들때 사용하는 api File
				//파일 객체를 활용해 파일의 크기를 계산할때 사용한다.
				File file = new File(fullPath);
				//위에서 받아온 파일의 바이트 정보를 담아준다.
				byte[] bytes = bs_file.getBytes();
				//파일을 쓰기 위한 객체 생성하기, 파라미터에 file이 들어있다.
				//그냥 FileOutputStream을 사용해도되지만 버퍼기능을 활용하기위해 BufferedOutputStream을 사용했다. 버퍼스트림은 단독으로 파일객체를 생성할 수없다. - 필터클래스
				BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream(file));
				//위에서 만든 출력객체를 활용해 읽어들인 파일 내용을 새로 생성한 File객체에 쓰기한다.(다운로드)
				bos.write(bytes);
				//사용한 출력스트림은 io에 대반 변조, 복제 등 보안을 이유로 반드시 닫아줘야한다.
				bos.close();
				
				//파일 크기
				//83번에서 생성한 파일 객체의 크기를 length함수를 통해 계산
				long size = file.length();
				//KB로 환산해 담는다.
				double d_size = Math.floor(size/1024.0);
				//@Reqeustparam의 타입인 Map에 파일명과 크기를 담는다.
				pMap.put("bs_flie", filename);
				pMap.put("bs_size", d_size);
			}catch(Exception e) {
				e.printStackTrace();
			}
		}
		logger.info("bs_file : "+bs_file+", pMap"+pMap.get("bs_file"));
		result = boardLogic.boardInsert(pMap);//첨부파일 여부는 logic에서 처리한다.
		if(result==1) {
			path = "redirect:boardInsertOk.jsp";
		}else {
			path = "redirect:boardInsertFail.sp";//Insert이기때문에 forward일 필요 없다.
		}
		return path;
	}
```

### 

