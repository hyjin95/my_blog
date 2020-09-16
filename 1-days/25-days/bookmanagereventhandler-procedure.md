# BookManagerEventHandler - procedure

## 기본 선언

```java
package design.book;
import java.awt.event.ActionEvent;
//main필요없이 interface에 actionlistener추가한다.
import java.awt.event.ActionListener;
import java.sql.CallableStatement;
import java.sql.Connection;
import java.sql.JDBCType;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.ArrayList;
import java.util.List;

import com.util.DBConnectionMgr;

import oracle.jdbc.OracleCallableStatement;
import oracle.jdbc.OracleTypes;

public class BookManagerEventHandler implements ActionListener {
	BookManager bm = null;//주의사항 : new금지. new는 복제본, 변하지 않은 원본을 가져와야한다.
	DBConnectionMgr dbMgr = DBConnectionMgr.getInstance();//con.util의 커넥션은 싱글톤 패턴
	Connection con = null;//물리적 거리가 있는 서버와 연결
	PreparedStatement pstmt = null;//SQL문 전달자
	ResultSet rs = null;//커서 조작
	
	CallableStatement cstmt = null;//procedure를 오라클 서버에 요청시 사용해야할 인터페이스
	OracleCallableStatement ocstmt = null;//ojdbc6.jar -> 오라클이 제공하는 API
```

## SELECT문

```java
//SELECT문 이용
	public List<BookVO> getBookList() {
		StringBuilder sql = new StringBuilder();
		List<BookVO> bList = new ArrayList<>();//List는 타입을 정해주어야 하기때문에 값이 여러 타입이라면 배열에 담아서 배열타입의 List에 담아야한다.
		sql.append("SELECT b_no, b_name, author, b_info, publish FROM book2020");
		try {
			con = dbMgr.getConnection();
			pstmt = con.prepareStatement(sql.toString());
			rs = pstmt.executeQuery();
			BookVO bVO = null;
			while(rs.next()) {
				bVO = new BookVO();
				bVO.setno(rs.getInt("b_no"));
				bVO.setname(rs.getString("b_name"));
				bVO.setAuthor(rs.getString("author"));
				bVO.setinfo(rs.getString("b_info"));
				bVO.setPublish(rs.getString("publish"));
				bList.add(bVO);
			}
			
		} catch (SQLException se) {//이 그물에 걸리는 에외는 모두 toad에서 잡히는 에러이다.
			System.out.println("[[query]]"+sql.toString());
		} catch (Exception e) {//그 외 나머지가 잡힌다.
			e.printStackTrace();//stack영역에 쌓여있는 에러 메세지의 이력을 라인번호와 함께 출력
		}
		return bList;
	}
```

## PROCEDURE

```java
//SELECT문이 아닌 procedure를 이용
	public List<BookVO> procBookList() {
		
		List<BookVO> bList = new ArrayList<>();//List는 타입을 정해주어야 하기때문에 값이 여러 타입이라면 배열에 담아서 배열타입의 List에 담아야한다.
		try {
			con = dbMgr.getConnection();
			cstmt = con.prepareCall("{call procBookList(?,?)}");//{ }자체가 파라미터이므로 ""를 붙인다. 파라미터에는 인자 값이 올 수 있다.=101번
			cstmt.registerOutParameter(1, OracleTypes.CURSOR);//파라미터는 커서타입이다.
			cstmt.registerOutParameter(2, JDBCType.VARCHAR);//자바타입의 경우
			cstmt.execute();//처리 요청
	        ocstmt = (OracleCallableStatement)cstmt;
	        ResultSet rs = ocstmt.getCursor(1);
			BookVO bVO = null;
			while(rs.next()) {
				bVO = new BookVO();
				bVO.setno(rs.getInt("b_no"));
				bVO.setname(rs.getString("b_name"));
				bVO.setAuthor(rs.getString("author"));
				bVO.setinfo(rs.getString("b_info"));
				bVO.setPublish(rs.getString("publish"));
				bList.add(bVO);
			}
		} catch (Exception e) {//그 외 나머지가 잡힌다.
			e.printStackTrace();//stack영역에 쌓여있는 에러 메세지의 이력을 라인번호와 함께 출력
		}
		return bList;
	}
```

