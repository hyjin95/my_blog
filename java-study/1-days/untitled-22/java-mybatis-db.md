# Java와 MyBatis - DB

## DBConnection.java : Java

```java
package movie_db;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;

public class DBConnectionMgr {
	public static DBConnectionMgr dbMgr = new DBConnectionMgr();
	public static final String _DRIVER = "oracle.jdbc.driver.OracleDriver";//ClassNotFoundException
	public static final String _URL    = "jdbc:oracle:thin:@192.168.0.187:1521:orcl11";
	public static String _USER 		   = "scott";
	public static String _PW           = "tiger";
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

## Configuration.xml : MyBatis

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
				<property name="url" value="jdbc:oracle:thin:@192.168.0.187:1521:orcl11" />
				<property name="username" value="scott" />
				<property name="password" value="tiger" />
			</dataSource>
		</environment>
	</environments>
	<mappers>
		<mapper resource="oracle/mybatis/emp.xml"/>
	</mappers>
</configuration>
```

* xml파일에 sql문을 관리하고, 18번 라인처럼 매핑하면, id로 접근할 수 있다.

