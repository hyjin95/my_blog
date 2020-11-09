# BandDao

역할 : DB연결과 Procedure호출 처리 클래스

## BandDao.java

```java
package net.tomato_step2;

import java.sql.CallableStatement;
import java.sql.Connection;
import java.sql.JDBCType;

import com.util.DBConnectionMgr;

import oracle.jdbc.OracleCallableStatement;

public class BandDao {
	//선언부
	Connection con = null;
	CallableStatement cstmt = null;
	OracleCallableStatement ocstmt = null;
	DBConnectionMgr dbMgr = DBConnectionMgr.getInstance();
	
	//로그인 처리 메서드 구현
	public String procLogin(String mem_id, String mem_pw) {
		String nickName = null;
		System.out.println(mem_id+mem_pw);
		System.out.println("procLogin 호출 성공");
		try {
			con = dbMgr.getConnection();
			cstmt = con.prepareCall("{call proc_login69(?,?,?)}");
			cstmt.setString(1, mem_id);//IN
			cstmt.setString(2, mem_pw);//IN
			cstmt.registerOutParameter(3, java.sql.Types.VARCHAR);;//OUT
			cstmt.executeUpdate();
			nickName = cstmt.getString(3);
		} catch (Exception e) {
			System.out.println(e.toString());
		}
		return nickName;
	}////////////////////////////////////end of procLogin//////////////////////////////////////
	
	public static void main(String args[]) {
		BandDao bdao = new BandDao();
		String nickName = bdao.procLogin("test", "123");
		System.out.println(nickName);
	}

}
```

