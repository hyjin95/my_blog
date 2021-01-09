---
description: 2020.12.04 ~ 진행중
---

# 파이널 프로젝트 - BoBeat

## 설계

### 의의

### 공정표 및 업무 분담

{% file src="../../.gitbook/assets/.xlsx \(1\).xlsx" caption="공정표, 업무분담 및 아이디어 회의" %}

### 화면 정의서

{% file src="../../.gitbook/assets/.xlsx \(2\).xlsx" caption="화면정의서" %}

## Solution

### Servlet

```java
package com.solution;

public class SolutionServlet extends HttpServlet {
	Logger logger = Logger.getLogger(SolutionServlet.class);

	public void doService(HttpServletRequest req, HttpServletResponse res)
			throws ServletException,IOException
	{
		logger.info("doService호출 성공");
		
		if(req.getLocalPort()==4444) {

			String requestURI = req.getRequestURI();
			String contextPath = req.getContextPath();// -> /
			String command = requestURI.substring(contextPath.length()+1);// solution/업무내용.solution
			int end = command.lastIndexOf(".");
			command = command.substring(0,end);// solution/업무내용
			//logger.info(command);
			
			Controller controller = null;
			String url = "/solution/";
			try {
				controller = ControllerMapper.getController(command);
			} catch (Exception e) {
				logger.info(e.toString());
			}
			
			if(controller instanceof SolutionController) {
				 logger.info("solutionController 호출성공");
		         ModelAndView mav = controller.execute_mav(req, res);
		         logger.info("mav.viewName :"+mav+", "+mav.viewName);
		         RequestDispatcher view = req.getRequestDispatcher(url+mav.viewName);
		         view.forward(req, res);
			}
		}
	}
	@Override
	public void doGet(HttpServletRequest req, HttpServletResponse res)
			throws ServletException,IOException
	{
		doService(req,res);
	}
	@Override
	public void doPost(HttpServletRequest req, HttpServletResponse res)
			throws ServletException,IOException
	{
		doService(req,res);
	}
}
```

* url요청을 받아 컨트롤러를 호출하는 서블릿 클래스입니다.
* 관리자만이 게시판을 생성 할 수 있기때문에 port번호가 관리자 port 번호인지 확인합니다.

### ControllerMapper

```java
package com.solution;

import org.apache.log4j.Logger;

public class ControllerMapper {
	Logger logger = Logger.getLogger(ControllerMapper.class);
	
	public static Controller getController(String command) {
		Controller controller = null;
		String commands[] = command.split("/");
		if(commands.length == 2) {
			String work = commands[0];//업무이름이 담길 변수
			String requestName = commands[1]; //메소드이름이면서 처리해야 할 업무이름이 담길 변수
			if("solution".equals(work)) {
				controller = new SolutionController(requestName);
			}
		}
		return controller;
	}
}
```

* url요청 중 업무이름이  solution이라면 solutionConroller로 매핑해주는 클래스입니다.

### Interface : Controller

```java
package com.solution;

public interface Controller {	

	public ModelAndView execute_mav(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException;	
}
```

* Controller계층 클래스들은 직접 작성한 Controller인터페이스를 상속받습니다.

### ModelAndView

```java
package com.solution;

//ModelAndView의 scope는 request이다.
public class ModelAndView {
	Logger logger = Logger.getLogger(ModelAndView.class);
	HttpServletRequest request = null;
	HttpServletResponse response = null;
	String viewName = null;
	List<Map<String,Object>> reqList = new ArrayList<>();
	
	public ModelAndView() {}
	
	public ModelAndView(HttpServletRequest request) {
		this.request = request;
	}
	
	public ModelAndView(HttpServletRequest request,HttpServletResponse response) {
		this.request = request;
		this.response = response;
	}
	
	public void setViewName(String viewName) {//응답페이지로 나갈 페이지 이름 결정하기
		logger.info("viewName : "+viewName);
		this.viewName = viewName;
	}
	
	public String getViewName() {
		return viewName;
	}
	
	public void addObject(String name, Object obj) {
		//여러개의 값을 추가하는 코드
		Map<String,Object> rMap = new HashMap<>();
		rMap.put(name,obj);
		reqList.add(rMap);	
		//아래 코드는 오직 한 개만 처리 가능
		request.setAttribute(name, obj);
		request.setAttribute("reqList", reqList);//scope가 request일때 값을 유지하기 session이아님
	}
}
```

* Spring에서 제공하는 ModelAndView 클래스를 직접 작성해 활용합니다.

### SolutionController

```java
package com.solution;

public class SolutionController implements Controller {
	Logger logger = Logger.getLogger(SolutionController.class);
	String requestName = null;
	SolutionLogic sLogic = null;
	
	public SolutionController(String requestName) {
		//requestName = board/boardList
		this.requestName = requestName;//응답페이지의 이름입니다.
	}

	@Override
	public ModelAndView execute_mav(HttpServletRequest req, HttpServletResponse res)
			throws ServletException, IOException {
		logger.info("execute 호출성공"+requestName);
		
		ModelAndView mav = new ModelAndView(req,res);
		sLogic = new SolutionLogic();
		
		//solution 페이지 처음 열릴떄
		if("startList".equals(requestName)) {
			
			List<Map<String,Object>> typeList = null;
			List<Map<String,Object>> titleList = null;
			
			typeList = sLogic.typeList();
			titleList = sLogic.titleList();
			
			logger.info(typeList.size());
			logger.info(titleList.size());
			
			mav.setViewName("solution_main.jsp");
			mav.addObject("typeList", typeList);
			mav.addObject("titleList", titleList);
		}
		
		//만들기 버튼을 눌렀을때
		else if("createBoard".equals(requestName)) {
			logger.info("execute - createBoard 호출성공"+requestName);
			
			Map<String,Object> pMap = new HashMap<>();
			String board_type = req.getParameter("board_type");
			pMap.put("board_type",board_type);
			String board_title = req.getParameter("board_title");
			pMap.put("board_title",board_title);
			String board_url = req.getParameter("board_url");
			pMap.put("board_url",board_url);
			String board_color = req.getParameter("board_color");
			pMap.put("board_color",board_color);
			
			logger.info(board_type+", "+board_title+", "+board_url+", "+board_color);
			
			int result = 0;
			result = sLogic.createBoard(pMap);
			
			if(1 == result) {
				mav.setViewName("main.jsp");
			}else {
				mav.setViewName("board_createFail.jsp");
			}
		}
		return mav;
	}
}
```

* 첫 페이지는 관리자계정에서 게시판 만들기 버튼을 클릭했을때 192.168.0.197:4444/solution/startList.solution 으로 서블릿을 거쳐 솔루션 페이지가 호출됩니다.
* 솔루션을 통한 게시판 만들기 정보를 담아 게시판 테이블을 생성하기 위한 로직 호출이 일어납니다.

### SolutionLogic

```java
package com.solution;

public class SolutionLogic {
	Logger logger = Logger.getLogger(SolutionLogic.class);
	
	SqlSolutionDao ssDao = null;
	
	//게시판 형태 typeList뽑아오는 메서드 - controller startList
	public List<Map<String,Object>> typeList() {
		//logger.info("typeList 호출성공");
		List<Map<String,Object>> typeList = null;
		ssDao = new SqlSolutionDao();
		typeList = ssDao.typeList();
		logger.info(typeList.size());
		return typeList;
	}

	//게시판 이름 titleList뽑아오는 메서드 - controller startList
	public List<Map<String, Object>> titleList() {
		//logger.info("typeList 호출성공");
		List<Map<String,Object>> titleList = null;
		ssDao = new SqlSolutionDao();
		titleList = ssDao.titleList();
		logger.info(titleList.size());
		return titleList;
	}
	
	//게시판 만들기 메서드 - controller createBoard
	public int createBoard(Map<String, Object> pMap) {
		logger.info("createBoard 호출성공");
		ssDao = new SqlSolutionDao();
		int result = 0;//insert 결과
		boolean result_b = true;//테이블 생성 결과
		boolean result_s = true;//시퀀스 생성 결과
		
		result_b = ssDao.createBoard(pMap);
		if(false == result_b) {//테이블 생성 성공시			
			result_s = ssDao.createSequence(pMap);
			if(false == result_s) {//시퀀스 생성 성공시
				result = ssDao.insertBoard(pMap);
			}
		}		
		return result;
	}
}
```

* Service계층에 해당하는 Logic클래스입니다.
* 업무별 알맞은 Dao 메서드를 호출합니다.

### SqlSolutionDao

```java
package com.solution;

public class SqlSolutionDao {
	Logger logger = Logger.getLogger(SqlSolutionDao.class);
	String resource = "com/util/Configuration.xml";
	SqlSessionFactory sqlMapper = null;
	
	DBConnectionMgr dbMgr   = null;
	Connection con 			= null;
	PreparedStatement pstmt = null;

	//MyBatis 솔루션 게시판 형식 가져오기
	public List<Map<String, Object>> typeList() {
		logger.info("typeList 호출 성공");
		List<Map<String,Object>> typeList = null;
		SqlSession session = null;		
		try {
			Reader reader = Resources.getResourceAsReader(resource);
			sqlMapper = new SqlSessionFactoryBuilder().build(reader,"development");
			//logger.info("sqlMapper"+sqlMapper);
			session = sqlMapper.openSession();
			typeList = session.selectList("typeList");
			//logger.info(typeList.size());
			session.close();
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			session.close();
		}
		return typeList;
	}
	
	//MyBatis 솔루션 게시판 이름 가져오기
	public List<Map<String, Object>> titleList() {
		logger.info("titleList 호출 성공");
		List<Map<String,Object>> titleList = null;
		SqlSession session = null;		
		try {
			Reader reader = Resources.getResourceAsReader(resource);
			sqlMapper = new SqlSessionFactoryBuilder().build(reader,"development");
			//logger.info("sqlMapper"+sqlMapper);
			session = sqlMapper.openSession();
			titleList = session.selectList("titleList");
			//logger.info(titleList.size());
			session.close();
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			session.close();
		}
		return titleList;
	}
	
	//JDBC 시퀀스 생성
	public boolean createSequence(Map<String,Object> pMap) {
		logger.info("createBoard 호출 성공");
		boolean result = true;
				
		dbMgr = DBConnectionMgr.getInstance();
		con = dbMgr.getConnection();
		StringBuilder sql = new StringBuilder();
		
		String name = pMap.get("board_url").toString();
		
		sql.append("CREATE SEQUENCE ");
		sql.append(name);
		sql.append("_seq");
		
		logger.info(sql.toString());
		try {
			pstmt = con.prepareStatement(sql.toString());
			result = pstmt.execute();
			logger.info(result);
		} catch (Exception e) {
			e.printStackTrace();
		} 
		return result;
	}

	//JDBC 테이블 생성
	public boolean createBoard(Map<String, Object> pMap) {
		logger.info("createBoard 호출 성공");
		boolean result = true;
				
		dbMgr = DBConnectionMgr.getInstance();
		con = dbMgr.getConnection();
		StringBuilder sql = new StringBuilder();
		
		String name = pMap.get("board_url").toString();
		
		sql.append("CREATE TABLE ");
		sql.append(name);
		sql.append(" (");
		sql.append("board_post_num number(37) constraints ");
		sql.append(name);
		sql.append("_num_pk primary key");
		sql.append(" ,board_post_title varchar2(100) not null");
		sql.append(" ,board_post_contents varchar2(4000) not null");
		sql.append(" ,mem_id varchar2(20)");
		sql.append(" ,post_price number(37) default 0");
		sql.append(" ,post_time varchar2(20)");
		sql.append(" ,post_own_cnt number(37) default 0");
		sql.append(" ,post_hit number(35) default 0");
		sql.append(" ,post_file_name varchar2(30)");
		sql.append(" ,post_file_size number(37) default 0");
		sql.append(" ,FOREIGN KEY (mem_id) REFERENCES bob_test.member_bob(mem_id))");
		logger.info(sql.toString());
		try {
			pstmt = con.prepareStatement(sql.toString());
			result = pstmt.execute();
			logger.info(result);
		} catch (Exception e) {
			e.printStackTrace();
		} 
		return result;
	}

	//JDBC 테이블 인서트
	public int insertBoard(Map<String, Object> pMap) {
	    logger.info("insertBoard 호출 성공");
		int result = 0;
		String seq_name = "master_seq.nextval";
		String board_type = pMap.get("board_type").toString();
		String board_title = pMap.get("board_title").toString();
		String board_url = pMap.get("board_url").toString();
		String board_color = pMap.get("board_color").toString();
		dbMgr = DBConnectionMgr.getInstance();
		con = dbMgr.getConnection();
		StringBuilder sql = new StringBuilder();
		
		String name = pMap.get("board_url").toString();
		
		sql.append("INSERT INTO board_master(BOARD_NUM, BOARD_TYPE, BOARD_TITLE, BOARD_URL, BOARD_COLOR) ");
		sql.append(" VALUES(");
		sql.append(seq_name+", ");
		sql.append("'"+board_type+"', ");
		sql.append("'"+board_title+"', ");
		sql.append("'"+board_url+"', ");
		sql.append("'"+board_color+"'");
		sql.append(" )");
		logger.info(sql.toString());
		try {
			pstmt = con.prepareStatement(sql.toString());
			result = pstmt.executeUpdate();
			logger.info(result);
		} catch (Exception e) {
			e.printStackTrace();
		} 
		return result;
	}
}
```

* 상황에 따라 Mybatis를 활용하기도, 기존의 JDBC를 사용하기도 합니다. 컬럼명에 변수가 들어가야할때 JDBC를 활용합니다. Create Table의 결과값을 boolean으로 받기위해 JDBC를 활용합니다.

### DBConnectionMgr

```java
package com.solution;

public class DBConnectionMgr {
	public static DBConnectionMgr dbMgr = new DBConnectionMgr();
	public static final String _DRIVER = "oracle.jdbc.driver.OracleDriver";//ClassNotFoundException
	//public static final String _URL    = "jdbc:oracle:thin:@192.168.0.39:1521:orcl";//메인
	public static final String _URL    = "jdbc:oracle:thin:@192.168.0.43:1521:orcl11";//test
	public static String _USER 		   = "bob_test";
	public static String _PW           = "bobeat";
	public Connection con = null;
	//싱글톤 패턴에 대해 생각해 보기
	public static DBConnectionMgr getInstance() {
		if(dbMgr==null) {
			dbMgr = new DBConnectionMgr();
		}
		return dbMgr;
	}
	//
	public Connection getConnection() {
		try {//예외처리를 하였다.실행시에 에러 발생할 가능성이 있는 코드는 try..catch안에 써준다.
			Class.forName(_DRIVER);
			con = DriverManager.getConnection(_URL, _USER, _PW);//인스턴스화
		} catch (ClassNotFoundException ce) {
			System.out.println("드라이버 클래스를 찾을 수 없습니다.");
		} catch (Exception e) {
			System.out.println("연결 실패!!!."+e.toString());
		}
		return con;//NullPointerException-인스턴스화 문제로 발생된다.
	}	
	//사용한 자원 반납하기
	//생성한 역순으로 한다.
	//생성하기 순서 - Connection - PreparedStatement - ResultSet
	public void freeConnection(Connection con
			                 , PreparedStatement pstmt
			                 , ResultSet rs) {
		if(rs!=null) {
			try {
				rs.close();				
			} catch (Exception e) {
				System.out.println("Exception ===> "+e.toString());
			}
		}
		if(pstmt!=null) {
			try {
				pstmt.close();				
			} catch (Exception e) {
				System.out.println("Exception ===> "+e.toString());
			}
		}
		if(con!=null) {
			try {
				con.close();				
			} catch (Exception e) {
				System.out.println("Exception ===> "+e.toString());
			}
		}
	}///////////////end of freeConnection
}
```

* DB연결과 자원관리를 담당하는 클래스입니다.
* 테스트는 테스트 서버에서 진행합니다.

### myBatis : SQL

```markup
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
 PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
 "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="oracle.mybatis.SolutionMapper">
	
	<select id="typeList" resultType="map">	   
		SELECT board_type FROM CATEGORY_BOB_CMS
	</select>
	
	<select id="titleList" resultType="map">	   
		SELECT board_title FROM BOARD_TITLE_CATEGORY GROUP BY board_title
	</select>
	
	<insert id="insertBoard" parameterType="map">
		INSERT INTO board_master(BOARD_NUM, BOARD_TYPE, BOARD_TITLE, BOARD_URL, BOARD_COLOR) 
    	    VALUES(#{seq_name},#{board_type},#{board_title},#{board_url},#{board_color})
	</insert>

</mapper>
```

* myBatis의 sql구문 관리 문서로 xml문서입니다.

### mybatis : configuration

```markup
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration
 PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
 "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
	<environments default="development">
		<environment id="development">
			<transactionManager type="JDBC" />
			<dataSource type="POOLED">
				<property name="driver" value="oracle.jdbc.driver.OracleDriver" />
				 <!-- <property name="url" value="jdbc:oracle:thin:@192.168.0.39:1521:orcl" /> -->
				 <property name="url" value="jdbc:oracle:thin:@192.168.0.43:1521:orcl11" />
				<property name="username" value="bob_test" />
				<property name="password" value="bobeat" />
			</dataSource>
		</environment>
	</environments>
	<mappers>
		<mapper resource="oracle/mybatis/solution.xml"/>
	</mappers>
</configuration>
```

* MyBatis의 경우 DB연결을 xml문서에서 관리합니다.

