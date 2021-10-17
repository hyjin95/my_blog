# MyProc.java 프로시저 테스트

### oracle 참조

![](<../../../.gitbook/assets/5 (7).png>)

* java 참조 클래스가 아닌 oracle 참조 클래스를 이용하려면, Build Path에 파일이 있어야만 사용가능하다.
* 소스파일 우클릭 - BuildPath - Configure Build Path - Libraries

## MyProc.java - 프로시저 테스트

### import

```java
package oracle.db;

import java.sql.CallableStatement;
import java.sql.Connection;
import java.sql.ResultSet;
import java.util.ArrayList;
import java.util.List;

import book.ch5.DeptVO;
import oracle.jdbc.OracleCallableStatement;
import oracle.jdbc.OracleTypes;

/*
 * 오라클의 프로시저를 자바코드에서 테스트하기
 * 자바와 오라클 연동시 SQL문을 전달할 수 있는 인터페이스가 있다.
 * 1)Statement - DML처리
 * 2)PreparedStatement - DML처리
 * 3)CallableStatement - PL/SQL처리
 */
```

* OracleCallableStatement 클래스는 PL/SQL 처리를 담당한다.\
  \- 오라클에서 생성한 객체(프로시저)를 오라클 서버에 전달해준다.
* oracle.jdbc 참조 클래스는 Build Path 라이브러리에 해당 클래스가 있어야 참조될 수 있다.
* java.sql.CallableStatement와 oracle.jdbc.OracleCallableStatement 둘다 import한 것은 서로 다른 메서드를 호출 하기 위함이다.

### 선언부

```java
public class MyProc {
	//물리적으로 떨어져있는 오라클 서버와 연결통로를 확보할때 필요한 선언 - 인터페이스이다.
	//왜냐하면 oracle제품, IP, PORT, SCOTT, TIGER, SID[orcl11] 을 결정할 수 없으므로
   Connection con = null;
   
   //오라클에서 생성한 객체(프로시저)를 오라클 서버에 전달해준다. 
   //역할 - 인터페이스 : 프로시저가 각각 다르므로 결정할 수 없다.
   CallableStatement cstmt = null;
   
   //자바에서 제공되는 커서는 오라클에서 제공되는 커서를 조작하는 것이므로 개선된 커서를 사용하기 위해 
   //마치 그래픽카드에 최신 드라이버를 설치하듯 새로운 인터페이스를 활용하는 것이다.
   //callableStatement는 java.sql 인터페이스고 
   //oracleCallableStatment는 orcle.jdbc 인터페이스이다.   
   OracleCallableStatement ocstmt = null;
   
   //재사용성을 높이고 일괄처리가 가능해진다. -> 서버이전 대비
   DBConnectionMgr dbMgr = new DBConnectionMgr();
```

* 14번 : oracle에서 지정된 개선된 커서, refCursor를 사용하기 위해 선언된 인터페이스

### PL/SQL 메서드

```java
   public List<DeptVO> deptList(){
      List<DeptVO> dList = new ArrayList<>();
      try {
         con = dbMgr.getConnection();
         
         //오라클 서버에 살고 있는 proc_dept객체를 자바 코드에서 호출하고 싶다면 
         //prepareCall메서드를 호출하세요.
         cstmt = con.prepareCall("{call proc_dept(?)}");
         
         //오라클 프로시저에서는 파라미터와 리턴을 따로 사용하지 않고 
         //모두 파라미터 형태로 사용한다. - 자바에서도 가능하다.
         //원본의 주소번지를 파라미터로 넘겨서 그 파라미터 주소번지를 활용하여 
         //리턴값을 꺼내는 형태로 지원한다. - 오라클 특징
         //오라클에서 제공하는 mode속성 : IN[사용자가 입력한 값을 받아올 때], 
         //OUT[사용자가 입력한 조건값으로 조회한 결과를 화면으로 내보낼때]
         //, INOUT(멀티로 사용하므로 편리할것같지만 사용하지 않는다.)
         cstmt.registerOutParameter(1, OracleTypes.CURSOR);
         
         //오라클서버에게 8번 프로시저 호출에 대한 처리를 요청하는 메서드 - execute
         cstmt.execute();
         ocstmt = (OracleCallableStatement)cstmt;
         ResultSet rs = ocstmt.getCursor(1);
         DeptVO dVO = null;
         while(rs.next()) {
            dVO = new DeptVO();
            dVO.setDeptno(rs.getInt("deptno"));
            dVO.setDname(rs.getString("dname"));
            dVO.setLoc(rs.getString("loc"));
            dList.add(dVO);
         }
      } catch (Exception e) {
         System.out.println(e.toString());
      }
      return dList;
   }
```

* 8번 : 프로시저 호출 메서드
* 오라클 프로시저는 리턴값도 파라미터에 넣는다.
* 20번 : 8번 처리 메서드

### Main 메서드

```java
   public static void main(String[] args) {
      MyProc mp = new MyProc();
      List<DeptVO> dList = mp.deptList();
      for(DeptVO rdVO:dList) {
         System.out.println(rdVO.getDeptno()+", "+rdVO.getDname()+", "+rdVO.getLoc());
      }
   }
}
```

* List를 for문으로 출력하기
