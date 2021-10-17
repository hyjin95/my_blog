# DBConnectionMgr.java

### 선언부

```java
package com.util;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;

public class DBConnectionMgr {
	
	public static DBConnectionMgr dbmgr = null;
	
	public static final String _DRIVER = "oracle.jdbc.driver.OracleDriver";
	public static final String _URL = "jdbc:oracle:thin:@192.168.0.187:1521:orcl11";
	public static String _USER = "scott";
	public static String _PW   = "tiger";
	Connection con = null;	
```

* 10번 : DBConnectionMgr은 public static으로 지정하여 공유할수 있는 싱글톤객체로 한다.
* oracle DRIVER주소와 ip와 이름이 담기는 URL은 재정의 할 수 없도로 final을 붙인다.
* 12-15번 : 모두 static, 싱글톤으로 한다.
* 16번 : DB와의 연결을 위해 Connection 클래스가 필요하다.

### 인스턴스화 메서드

```java
	//싱클톤 패턴에 대해 생각해보기
	public static DBConnectionMgr getInstance() {//static이 붙으면 바로 사용가능하다.
		if(dbmgr==null) {//인스턴스화안되어있으면 넘긴다.
			dbmgr = new DBConnectionMgr();
		}
		return dbmgr;//return object타입
	}
```

* 인스턴스화 메서드, DBConnecionMgr타입이다. 리턴타입으로 Object타입을 갖는다.

### 예외처리 메서드

```java
	public Connection getConnection() {//Connection import, 참조형이다, 연결
		try {//예외처리를 했다. 실행시에 에러발생할 가능성이있는 코드는 try-catch로 예외처리한다.
			Class.forName(_DRIVER);
			con = DriverManager.getConnection(_URL, _USER, _PW);
		} catch (ClassNotFoundException ce) {
			System.out.println("드라이버 클래스를 찾을 수 없습니다.");
		} catch (Exception e) {
			System.out.println("연결 실패 "+e.toString());
		}
		return con;//return 반환타입이 인터페이스이다.
	}	
```

* Connection 타입으로 리턴타입으로 Object타입을 갖는다.
* 3번 : 드라이버 클래스 이름을 찾는 호출
* 4번 : URL, USER, PW로 연결을 하는 호출 = 인스턴스화
* 5,6번 : 3번에서 클래스 이름을 찾을 수 없는 예외처리
* 7,8번 : 4번에서 con, 연결 오류에 대한 예외처리.
* 10번 : 반환타입이 null이라면 NullPointException오류가 난다.

### 자원반납 메서드

```java
	//서버의 입장에서는 사용한 자원은 반드시 반납해야한다.
	//생성한 역순으로 한다.
	//순서 : Connection - PreparedStatment - ResultSet 
	public void freeConnection(Connection con
								, PreparedStatement pstmt
								, ResultSet rs) {
		if(rs!=null) {
			try {
				rs.close();
			}catch (Exception e) {
				System.out.println("Exception ---> "+e.toString());
			}
		}//end of if
		if(pstmt!=null) {
			try {
				pstmt.close();
			}catch (Exception e) {
				System.out.println("Exception ---> "+e.toString());
			}
		}//end of if
		if(con!=null) {
			try {
				con.close();
			}catch (Exception e) {
				System.out.println("Exception ---> "+e.toString());			
			}
		}//end of if		
	}//end of freeConnection
}//class
```

* 서버의 입장에서는 사용한 자원은 반드시 반납되어야한다.
* 생성한 역순으로 한다.\
  \- Connection -> PreparedStatment -> ResultSet
* 4-6번 : 사용할 함수 인스턴스화

