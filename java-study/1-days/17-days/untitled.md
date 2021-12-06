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
			pstmt.setNString(1,  dong+"%");
			rs = pstmt.executeQuery();
			ZipCodeVO zVO = null;
			Vector    v   = new Vector();
			while(rs.next()) {
				zVO = new ZipCodeVO();
				zVO.setZipcode(rs.getInt("zipcode"));
				zVO.setAddress(rs.getString("address"));
				v.add(zVO);
			}
			zVOS = new ZipCodeVO[v.size()];//v.size = add한 갯수만큼
			v.copyInto(zVOS);//복사메서드
		}catch (Exception e) {				
		}
		return zVOS;
	}//end of getZipCodList
}//end of class
```

* ZipCodeVO\[ ]타입인 getZipCodeList메서드는 String타입 dong을 파라미터로 갖고있다.\
  \- dong으로 조회하는 메서드이다.
* 3번 : zVOS\[ ]배열은 조회할때마다 다른 값을 받으므로 지역변수로 생성하되 어느 동 우편번호를 조회할 것인지 정해지지 않았으므로 null로 한다.
* 4번 : DBConnectionMgr의 getInstance메서드는 static이므로 바로 호출가능하다.
* 6번 : 참고 : 11Days - StringBuilder
* 7-9번 : 문자열을 더할때 +가 아닌 append를 사용한다.\
  \- oracle 에서 불러오는 것으로 oracle에 쓰인대로 적혀야 제대로 호출이 된다.\
  \- 띄어쓰기를 항상 주의하자
* 9번에 변수를 넣는 방법1\
  \-** **sql.append(" WHERE dong LIKE "+"'"+dong+"%'"); 하거나\
  \- sql.append("'"+dong+"%'"); 을 밑줄에 추가한다.
* 9번에 변수를 넣는 방법2\
  \- 15번 참고
* 12-30번 : 예외처리
* 13번 : DBConnectionMgr의 Connnection을 가져다 쓴다.
* 15번 : 첫번째 컬럼명에 입력되는 dong이 포함된 단어를 조회한다.
* 16번 : 찾은, SELECT한 값을 rs에 담는다.
* 17,18번 : 찾은 데이터를 담을 객체배열\[벡터 size] 선언
* 19번 : 다음 row로 넘어간다. = 주소를 계속 새로 기억시킨다.//반
* 20번 : 데이터를 담을 배열을 생성한다.
* 21,22번 : 찾은 zipcode, address를 저장한다.
* 23번 :  Vector에 담는다. //반복
* 25번 : 반복문 밖에서 데이터를 담을 객체 배열\[Vector size]을 선언한다.
* 26번 : Vector에 담긴 값을 zVOS객체 배열에 불어넣는다.
* 27번 : 오류가 일어나면\
  \- ex) 화면에 로딩화면이 들고있다 = 죽진않았다. 멈춘상태. -> 무언가 반복하고있다
* 29번 : ZipCodeVO\[ ]타입 getZipCodeList(String dong)메서드의 반환값은 zVOS이다.

## ZipCodeDaoSimulation.java

```java
package desigin.test;

public class ZipCodeDaoSimulation {
	
	public static void main(String[] args) {
		ZipCodeDao zcDao = new ZipCodeDao();
		ZipCodeVO[] zVOS = zcDao.getZipCodeList("당산동");
		for(ZipCodeVO zVO:zVOS) {
			int zipcode = zVO.getZipcode();
			String address = zVO.getAddress();//불러올 정보의 타입을 맞춘다.
			System.out.println(zipcode+"-"+address);
		}
	}
}
```

* 6번 : 인스턴스화
* 7번 : 값이 담길 객배열 선언, ZipCodeDao클래스에서 입력된 값에 대한 값을 받는다.
* 8-12번 : zVOS의 값을 모두  보여줘 : (객체배열 안에 들어있는 타입 : 변수이름)\
  \- zVOS객체 배열안에는 VO타입들이 담겨있다.
* 9번 : int타입의 zipcode는 zVO의 zipcode를 읽어온다. \
  \- zVO에 담긴 zipcode데이터와 새로 담을 변수zipcode의 타입이 같아야 한다.
* 10번 : String 타입의 address는 zVO의 address를 읽어온다.\
  \- zVo에 담긴 address데이터와 새로 담을 변수 address는 타입이 같아야한다.
* 11번 : 출력 //반

## ZipCodeVo.java - get, set

### 선언부

```java
package desigin.test;

public class ZipCodeVO {
	private int    uid_no   = 0; 
	private int    zipcode  = 0;
	private String zdo      = null;
	private String sigu     = null;
	private String dong     = null;
	private String ri       = null;
	private String bungi    = null;
	private String aptname  = null;
	private String upd_date = null;
	private String address  = null;
	public ZipCodeVO() { }
	public ZipCodeVO(int uid_no, int zipcode, String zdo, String sigu, String dong, String ri
			                 , String bungi, String aptname, String upd_date, String address) { 
		this.uid_no   = uid_no;
		this.zipcode  = zipcode;
		this.zdo      = zdo;
		this.sigu     = sigu;
		this.dong     = dong;
		this.ri       = ri;
		this.bungi    = bungi;
		this.aptname  = aptname;
		this.upd_date = upd_date;
		this.address  = address;
	}	
```

* 4-13번 : toad에 입력된 모든 header명을 선언해야한다.\
  \- number 타입은 int타입으로, varchar2타입은 String타입으로 선언한다.
* 15번 : 컬럼명들을 파라미터로 갖는 생성자를 생성한다. 파라미터에 모든 컬럼명이 들어가야한다.
* 17-26번 : 파라미터 변수들을 멤버변수에 담는다.

### getter, setter함수

```java

	public int getUid_no() {
		return uid_no;
	}
	public void setUid_no(int uid_no) {
		this.uid_no = uid_no;
	}
	public int getZipcode() {
		return zipcode;
	}
	public void setZipcode(int zipcode) {
		this.zipcode = zipcode;
	}
	public String getZdo() {
		return zdo;
	}
	public void setZdo(String zdo) {
		this.zdo = zdo;
	}
	public String getSigu() {
		return sigu;
	}
	public void setSigu(String sigu) {
		this.sigu = sigu;
	}
	public String getDong() {
		return dong;
	}
	public void setDong(String dong) {
		this.dong = dong;
	}
	public String getRi() {
		return ri;
	}
	public void setRi(String ri) {
		this.ri = ri;
	}
	public String getBungi() {
		return bungi;
	}
	public void setBungi(String bungi) {
		this.bungi = bungi;
	}
	public String getAptname() {
		return aptname;
	}
	public void setAptname(String aptname) {
		this.aptname = aptname;
	}
	public String getUpd_date() {
		return upd_date;
	}
	public void setUpd_date(String upd_date) {
		this.upd_date = upd_date;
	}
	public String getAddress() {
		return address;
	}
	public void setAddress(String address) {
		this.address = address;
	}	
}
```

* SIUD 작업을 위한 get, set 함수들을 만들어야한다.
* 우클릭 -> Source -> Generate Getter and Setter -> select all -> Lastmember -> Generate
