---
description: 2021.01.04 - 96일차
---

# 96 Days - spring:삭제, dataSet, 첨부파일코드, required, Android:Activity생명주기, Handler, Thread, StopWatch

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org, Toad DBA Suite for Oracle 11.5 , Spring, Android Studio
* 사용 서버 - WAS : Tomcat

## 필기

### UI

* ECMAScript - ES5, ES6
* React.js
* Vue.js - JS기반, 뷰에 대한 템플릿을 지원한다. - 네이버 카페처럼 2단, 3단 구성의 템플릿으로 빠르고 쉽게 그릴 수 있도록한다.
* TypeScript  - JS를 포함하는 스크립트 - JS코드를 지원하면서 JS에서는 제공하지 않던 타입 체크라던지 객체 지원이 다양하다.

### 자료구조

* 자료구조 = 컬렉션프레임워크 = 파이썬\(리스트\(자바 list\), 튜플\(상수 리스트:순서는 있지만 변경불가\), 딕셔너리\(자바 map\), 집합{자바 set\) : 자료의 도식화
* json\(web,안드로이드0, 넥사크로\(xml그리고 js변환해줌\) - DataSet지원, Post방식이 기본 

## Spring-boot

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

### FrameWork의 사용 의의

* 반복되는 코드를 줄일 수 있다.
* 예시1 \) 사용자 입력값 받아오기 - spring-boot 프레임워크의 @RequestParam 어노테이션을 사용한다. - 기존 반복 코드\(POJO\) : request.getParameter\(여기만 다르다\)
* 예시2\) select -&gt; forward로 데이터 유지 - Spring 제공클래스 Model, ModelMap지원 : ui를 지원하기위한 api - POJO : request.setAttribute\("이름",값\); 

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
```

* 파라미터의 값이 null일경우를 고려하면 required옵션을 false로 두어야 한다. not null이 아닌 값인 경우

```java
		if(bs_file != null) {//첨부파일이 있다면
			filename = bs_file.getOriginalFilename();//본래의 파일 명을 가져온다.
			fullPath = pathSave+"\\"+filename;//물리적 경로와 파일 명을 연결해 위치정보를 다시 저장한다.
		}
```

* 첨부 파일이 존재한다면 본래 파일명을 가져와 물리적 경로와 합쳐 위치정보를 작성한다.
* 파일 이름을 담는다.

```java
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
```

* 첨부파일의 이름을 통해 파일 객체화 한다. 객체화 한 물리적 위치에 저장된 파일을 읽어옴으로서 크기를 읽을 수 있다.

```java
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

## Android Studio : Activity 생명주기

### 학습내용

* 액티비티가 생성되고 파괴될 때 어떤 일이 발생하는가
* 액티비티의 상태를 저장하고 복원하는 방법
* 라이프사이클을 이해하면 인터셉트\(끼어들기\)하는 부분을 포착해 작업을 할 수 있다. 관여할 수 있게 된다.
* 단말기를 회전시키는 순간 Activity설정이 초기화되어 버린다.

  진행중이던 저장되지 않은 정보들이 사라져 버리고 다시 초기값을 불러오게 된다.

  진행중이던 정보를 유지하려면 lifeCycle을 활용해야한다. onStop\( \)메서드가 호출되어 정지되었을때 어떻게 정보를 유지할 것인가

### 하이브리드 앱

* 웹앱 + 네이티브앱
* 웹앱은 단독으로는 실행이 불가능하고 인터넷 연결이 필수조건이다. 디바이스의 고유 하드웨어인 카메라 같은 기능을 제어, 활용이 불가능하다.
* 네이티브 앱은 카메라, 위치, 근거리통신망과 같은 기능들을 활용할 수 있다. Anroid의 SHA1 코드와 같은 식별자들을 사용한다. 스토어에 등록할 수 있고 단말기에서 지원하는 noSQL제품을 사용할 수 있다. SQLite : 단말기 지원 DB로 로컬 DB를 활용할 수 있다.

### Activity 생명주기

* 액티비티 시작 - 액티비티 실행 - 액티비티 종료
* onCreate\( \) - onStart\( \) - onResume\( \) - onPause\( \) - onStop\( \) - onDestroy\( \)

{% page-ref page="../95-days/" %}

### 작업일지

1. 프로젝트 등록
2. AndroidX버전으로 conversion
3. 화면정의 - 시간정보 출력 : TextView - 시작, 정지, 리셋 : Button
4. 매 초 seconds 변수값을 증가시키고 텍스트뷰를 갱신 할 수 있도록 반복수행 시킨다. - 메인 스레드와 UI스레드가 별개이므로 메인스레드에서 UI를 조작하는 것은 불가능하다. - Thread를 구현해 해결한다.  - Handler, Volley API 활용 - 다른 스레드에서 사용자 인터페이스를 갱신하려면 예외가 발생하므로 메인 스레드에서    갱신하도록 처리해야한다. Handler API를 제공하는 이유가 바로 여기에 있다. - Handler를 사용하면 특정코드를 미래 시점에 수행되도록 할 수 있다.   매 초 스탑워치 코드를 실행한다.

### Handler

```java
final Handler handler = new Handler();
handler.postDelayed(Runnable, long);
```

* post\( \) : 즉시실행
* postDelay\(스레드, long\) : 지연, 미래시점에서 실행
* 작업스레드에서 사용자 인터페이스에 접근, 갱신할때 사용된다.

### Thread

* 메인 액티비티가 메인 스레드이므로 이 내부에서는 UI를 조작할 수 있다. 작동 시간의 지연, 중지, 와 같은 시간 제어의 작업도 스레드 개념안에서 이루어진다.
* Activity에 연결되어 있는 xml에 접근하는데는 내부적으로 별도의 스레드 없이도 접근이 가능하지만

  다른 Activity에서 다른 Activity의 xml에 접근할때에는 스레드를 구현해야한다.

* 동시 접속자가 있어 경합이 발생할 때 필요하다. 대기실에서 부터 순서를 정해준다.
* 자바에서는 - extends Thread - implemnets Runnable - thread를 생성해 thread.start\( \)하면 run\( \)메서드가 호출된다.   start를 호출하더라도 순서에 따라 진행된다.

{% page-ref page="../30-days/" %}

### StopWatchBefore.apk

* Life Cycle 반영 이전
* 화면을 회전시키면 모두 초기화 되어버린다.

{% page-ref page="android-studio.md" %}

### StopWatchAfter.apk

* Life Cycle을 고려한 APK

후기 : 으으ㅡ으으 추워ㅓ

