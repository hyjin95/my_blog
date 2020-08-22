# ZipCode.java

## ZipCodeDap.java

### 선언부

```java
package desigin.test;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.util.Vector;

import com.util.DBConnectionMgr;

public class ZipCodeDao {//조회하기
	DBConnectionMgr   dbmgr = null;
	Connection        con   = null;
	PreparedStatement pstmt = null;
	ResultSet         rs    = null;
```

* 참고 : DBConnectionMgr.java

### dong 조회 메서드

```java
	public ZipCodeVO[] getZipCodeList(String dong) {
	//dong이 파라미터로 있어야 조회할 수 있다.
		ZipCodeVO zVOS[] = null;
		dbmgr = DBConnectionMgr.getInstance();
		
		StringBuilder sql = new StringBuilder();
		sql.append("SELECT zipcode, address");
		sql.append(" FROM   zipcode_t      ");
		sql.append(" WHERE  dong LIKE ? ");
		//dong이 정해지지 않았으므로 ?처리, 테스트할때에는 임의 동을 넣자.

		try {
			con = dbmgr.getConnection();
			pstmt = con.prepareStatement(sql.toString());
			pstmt.setNString(1,  dong+"%");//변수를 넣는방법2
			rs = pstmt.executeQuery();
			ZipCodeVO zVO = null;
			Vector    v   = new Vector();
			while(rs.next()) {//주소를 계속 새로 기억시킨다.
				zVO = new ZipCodeVO();
				zVO.setZipcode(rs.getInt("zipcode"));
				zVO.setAddress(rs.getString("address"));
				v.add(zVO);
			}
			zVOS = new ZipCodeVO[v.size()];//v.size = add한 갯수만큼
			v.copyInto(zVOS);//복사메서드
		}catch (Exception e) {//화면에 로딩화면이 들고있다 = 죽진않았다. 멈춘상태. -> 무언가 반복하고있다.
			
		}
		return zVOS;
	}//end of getZipCodList
}//end of class
//중요한 코드 : 28,29,31,32,33
```

* ZipCodeVO\[ \]타입인 getZipCodeList메서드는 String타입 dong을 파라미터로 갖고있다. - dong으로 조회하는 메서드이다.
* 3번 : zVOS\[ \]배열은 조회할때마다 다른 값을 받으므로 지역변수로 생성하되 어느 동 우편번호를 조회할 것인지 정해지지 않았으므로 null로 한다.
* 4번 : DBConnectionMgr의 getInstance메서드는 static이므로 바로 호출가능하다.
* 6번 : 참고 : 11Days - StringBuilder
* 7-9번 : 문자열을 더할때 +가 아닌 append를 사용한다. - oracle 에서 불러오는 것으로 oracle에 쓰인대로 적혀야 제대로 호출이 된다. - 띄어쓰기를 항상 주의하자
* 9번에 변수를 넣는 방법1 - ****sql.append\(" WHERE dong LIKE "+"'"+dong+"%'"\); 하거나 - sql.append\("'"+dong+"%'"\); 을 밑줄에 추가한다.
* 9번에 변수를 넣는 방법2 - 15번 참고

