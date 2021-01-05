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

### 필기

* xxx.APK로 묶인 것을 네이티브 앱이라하고 네이티브 앱은 스토어에 올릴 수 있으며 디바이스에서 사용할 수 있다.
* 시작Activity는 MainActivity로서 Activity클래스가 아닌 하위 클래스인 AppCompatActivity를 상속받아 사용해야 더 많은 기능을 활용 할 수 있다.

### StopWatchAfter.apk

* 현재상태를 저장하기 위해 메서드를 오버라이드 한다. - onSaveInstanceState\(Bundle savedInstanceState\) - 메서드 안에서 반드시 부모\(상위\)메서드를 불러와야 한다.
* Bundle, 번들에는 여러 종류의 데이터를 한 객체로 저장가능하다.
* 저장해야 되는 정보 - 시간\(int\) - 실행 상태\(boolean\) - 이전 실행 상태\(boolean\)
* Life Cycle을 고려한 APK onStop함수가 호출되기 직전에 onSaveInstanceState\( \)메서드를 호출한다.
* 화면이 회전되더라도 \(액티비티가 새로 시작되더라도\) 상태가 유지된다.

### 1. 상태값을 저장하는 메서드

```java
public class MainActivity extends AppCompatActivity {

    private int seconds = 0;
    private boolean running = false;//실행상태
    
    //과거의 정보를 담을 변수
    private boolean wasRunning = false;

    //세로에서 가로로 화면 전환시 현재 상태를 저장 -> 재사용
    //번들에는 여러 종류의 데이터를 한 객체로 저장이 가능하다.
    @Override
    public void onSaveInstanceState(Bundle savedIntanceState) {
        super.onSaveInstanceState(savedIntanceState);
        savedIntanceState.putInt("seconds", seconds);//첫번째 저장값
        savedIntanceState.putBoolean("running", running);//두번째 저장값
        savedIntanceState.putBoolean("wasRunning", wasRunning);//세번째 저장값
        //여기까지 이전상태를 저장하는 코드 추가 
    }
```

* Bundle객체에는 여러 타입의 데이터를 담을 수 있다.
* 13번에서 상위메서드롤 불러온다.
* 14-16번에서 번들에 데이터를 저장한다.

### 2. 상태값을 변경하는 메서드

```java
    //상태값을 변경할 수 있는 함수 --2
    public void onStop() {//액티비티가 멈췄을때
        super.onStop();
        //현재 상태를 저장
        wasRunning = running;
        //현재 상태는 초기화
        running = false;
    }

    //디바이스가 누웠을때 새로 액티비티가 생성되는데 화면에 출력되기전에 이전 상태 정보를 가져와 계속 1씩 증가하여 출력을 내보내야 한다.
    //이때 0에 1을 더하는 것이 아닌 이전 상태를 저장해둔 값에 +1 해야 할 것이다.
    //running상태는 최초에 false이므로 초기화를 해주게되면 처음상태로 상태정보를 바꿀 수 있다. -라이프사이클 컨트롤하기

    public void onStart() {//액티비티가 시작될때--2
        super.onStart();
        if(wasRunning) {//과거 상태정보가 true였다면
            running = true;//상태정보 변환
        }
    }
```

### 3. 상태값 불러오기

```java
@Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        //시간정보를 출력하기 전에 Bundle에서 값을 얻어와 액티비티 상태값을 복원해야한다.--3
        if(savedInstanceState != null) {//null일때만 초기화해주면 됨
            seconds = savedInstanceState.getInt("seconds");
            running = savedInstanceState.getBoolean("running");
            wasRunning = savedInstanceState.getBoolean("wasRunning");
        }
        //새로운 액티비티가 생성되었으므로 다시 호출된다.
        runTimer();
    }
```

