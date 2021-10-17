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
	BookManager bm = null;//주의사항 : new는 복제본, 변하지 않은 원본을 가져와야한다.
	DBConnectionMgr dbMgr = DBConnectionMgr.getInstance();//con.util의 커넥션은 싱글톤 패턴
	Connection con = null;//물리적 거리가 있는 서버와 연결
	PreparedStatement pstmt = null;//SQL문 전달자
	ResultSet rs = null;//커서 조작
	
	CallableStatement cstmt = null;//procedure를 오라클 서버에 요청시 사용해야할 인터페이스
	OracleCallableStatement ocstmt = null;//ojdbc6.jar -> 오라클이 제공하는 API
```

* com.util에 있는 DBConnectionMgr클래스는 싱글톤 패턴이므로 getInstance함수를 이용한다.
* Connection : 서버와의 물리적 거리를 연결 하기 위함
* PreparedStatement : SQL문 전달자
* ResultSet : 커서 조작 메서드 제공
* CallableStatement : 자바 제공 API, 프로시저 전달자 인터페이스
* OracleCallableStatement : 오라클 제공 API, 전달자 인터페이스

## SELECT문 이용

```java
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

* **타입이 List\<BookVO>**인 메서드
* ArrayList와 SELECT문을 이용한 sql이다.
* SELECT문 이므로 executeQuery함수를 이용한다.
* ResultSet의 next함수로 기본 커서를 조작한다.
* 19번 : sql문에 대한 예외처리

## PROCEDURE 이용

```java
public List<BookVO> procBookList() {		
	List<BookVO> bList = new ArrayList<>();
	try {
		con = dbMgr.getConnection();
		//{ }자체가 파라미터이므로 ""를 붙인다. 파라미터에는 인자 값이 올 수 있다.=8번
		//cstmt = con.prepareCall("{call procBookList(?,?)}");
		cstmt = con.prepareCall("{call procBookList(?)}");
		cstmt.registerOutParameter(1, OracleTypes.CURSOR);//파라미터는 커서타입이다.
		//cstmt.registerOutParameter(2, JDBCType.VARCHAR);//자바타입의 경우
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

* **타입이 List\<BookVO>**인 메서드
* 5번 : 프로시저 PL/SQL문, { }자체가 파라미터이다.\
  \- 파라미터가 하나이다.(? 갯수)
* 8번 : OUT(반환) 파라미터는 CURSOR타입이다.
* 10번 : 프로시저 실행 : execute함수이용
* 11번 : cstmt를 오라클 제공 인터페이스 타입으로 형전환한다.
* 일반 SQL문이아닌 프로시저 호출이므로 SQL문 예외처리는 필요없다.
