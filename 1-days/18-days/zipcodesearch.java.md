# ZipCodeSearch.java - 우편번호 조회

### +NickNameListSub.java

dong을 검색하면 우편번호와 주소를 보여주는 창을 구현하려고 한다.

```java
package desigin.test;

import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

public class NickNameListSub extends JDialog implements ActionListener{
	NickNameList nnl     = null;
	JScrollPane  jspLine = null;

	JPanel      jp_center   = new JPanel();
	JLabel      jlb_zipcode = new JLabel("우편번호");
	JTextField  jtf_zipcode = new JTextField(6);
	JLabel      jlb_address = new JLabel("주소");
	JTextField  jtf_address = new JTextField(30);

	public void initDisplay() {

		jp_center.setLayout(null);//레이아웃 뭉개기

		jlb_zipcode.setBounds(20, 45, 60, 20);
		jtf_zipcode.setBounds(90, 45, 80, 20);
		jlb_address.setBounds(20, 70, 100, 20);
		jtf_address.setBounds(90, 70, 170, 20);
		jbtn_zipcode.setBounds(170, 45, 60, 19);
		
		jbtn_zipcode.addActionListener(this);
		
		jp_center.add(jbtn_zipcode);
		jp_center.add(jlb_zipcode);
		jp_center.add(jtf_zipcode);
		jp_center.add(jlb_address);
		jp_center.add(jtf_address);
		jspLine = new JScrollPane(jp_center);//위에서 라벨과 입력창의 위치를 결정한 후에, 사용되기 전에 생성되어야 한다.

	@Override
	public void actionPerformed(ActionEvent e) {
		// TODO Auto-generated method stub		
	}
}//end of class
```

* 15 Days - NickNameListSub.java에서 추가된 부분

## ZipCodeSearch.java

#### 우편번호 검색시 자료보여주기, 참고 : ZipCodeVO.java

### import

```java
package desigin.test;

import java.awt.BorderLayout;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.FocusEvent;
import java.awt.event.MouseEvent;
import java.awt.event.MouseListener;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.List;
import java.util.Vector;

import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JPanel;
import javax.swing.JScrollPane;
import javax.swing.JTable;
import javax.swing.JTextField;
import javax.swing.table.DefaultTableModel;

import com.util.DBConnectionMgr;
```

### 선언부

```java
public class ZipCodeSearch extends JFrame implements MouseListener, ActionListener {
	//선언부
	//물리적으로 떨어져있는 db서버와 연결통로 만들기
	Connection        con   = null;
	//위에서 연결되면 쿼리문을 전달할 전령 역할의 인터페이스 객체 생성
	PreparedStatement pstmt = null;//insert, update, delete에 필요
	//조회된 결과를 화면에 처리해야하므로 오라클에서 커서를 조작하기 위한 인터페이스 추가 
	ResultSet         rs    = null;//select에서만 필요
	
	JPanel     jp_north    = new JPanel();
	JTextField jtf_search  = new JTextField("동 이름을 입력하세요.");
	JButton    jbtn_search = new JButton("조회");
	
	String cols[]   = {"우편번호", "주소"};
	String data[][] = new String[0][2];//0번째, 컬럼 2개
	DefaultTableModel dtm_zipcode = new DefaultTableModel(data,cols);//데이터 얹기
	JTable jtb_zipcode = new JTable(dtm_zipcode);//양식제공
	JScrollPane jsp_zipcode = new JScrollPane(jtb_zipcode
											  , JScrollPane.VERTICAL_SCROLLBAR_AS_NEEDED
											  , JScrollPane.HORIZONTAL_SCROLLBAR_AS_NEEDED);//스크롤바 제공
```

* ZipCodeSearch클래스는 JFrame을 \(extends\)상속받고, MouseListener, ActionListener 인터페이스를 implements 한다.
* 3-8번 : DB연동에 필요한 인터페이스를 선언한다.
* 10-20번 : 화면에 필요한 것들을 생성한다.

### 화면구현부

```java
	//생성자
	public ZipCodeSearch() {
		initDisplay();
	}
	
	//화면처리부
	public void initDisplay() {
		jbtn_search.addActionListener(this);
		jtf_search.addMouseListener(this);
		
		jp_north.setLayout(new BorderLayout());
		jp_north.add("Center", jtf_search);
		jp_north.add("East", jbtn_search);
		
		this.add("Center", jsp_zipcode);
		this.add("North", jp_north);
		this.setTitle("우편번호 검색");
		this.setSize(430, 400);
		this.setVisible(true);
	}//end of initDisplay
	
	//메인메서드
	public static void main(String[] args) {
		ZipCodeSearch zcs = new ZipCodeSearch();
	}//end of main	
```

* 디폴트 생성자, 기본화면구현, 메인메서드

### 조회 메서드 구현

#### 설정, 선언 부분

```java
	//조회메서드
	public void refreshData(String dong) {
		DBConnectionMgr dbMgr  = DBConnectionMgr.getInstance();
		con = dbMgr.getConnection();
		StringBuilder sql = new StringBuilder();
		sql.append("SELECT zipcode, address");
		sql.append(" FROM zipcode_t");
		sql.append(" WHERE 1=1");		
```

* 2번 : 조회메서드인 refrechData메서드는 String타입 파라미터를 하나 갖는다.
* 3번 : 연결통로는 17 Days - DBConnectionMgr을 이용한다. dbmgr의 인스턴스화 메서드 호출
* 4번 : 내 클래스의 con 에 대입
* 사용되는 sql문은 StringBuilder타입으로 한다.
* 6-8번 : 띄어쓰기 주의하기

#### 검색조건

```java
		if(dong!=null) {
			sql.append(" AND dong LIKE '%'||?||'%'");//전체검색
		}		
```

* dong이 비어있는 값이 아니라면, sql에 전체검색 조건을 append한다.
* "'%' \|\| ? \|\| '%'" : ?가 포함된 모든 데이터

#### 예외처리 - 선언부, try

```java
		try {
			pstmt = con.prepareStatement(sql.toString());
			pstmt.setString(1, dong);
			rs = pstmt.executeQuery();
		
			//Vector v = new Vector();
			Vector<ZipCodeVO> v = new Vector<>();//권장사항
			//List v2 = new Vector(); 
			//같다, 선언부에 인터페이스 하지만 사용하지 않는다.
			ZipCodeVO zVOS[] = null;
			ZipCodeVO zVO = null;
```

* 파라미터로 넘어온 select문을 스캔하여 ?의 갯수를 파악한다. pstmt.setString\( \)의 파라미터로 설정한다.
* 2번 : 검색문을 전달해줄 pstmt 생성
* 3번 : 검색조건 \(?갯수, 찾는조건\)
* 4번 : 검색하는 rs 생성
* 7번 : 값을 담을수 있는 Vector 배열 생성
* 8번 : List인터페이스를 사용해도되지만 후에 사용해야하는 copyinto를 사용할 수 없게된다.
* 10-11번 : 객체배열, 배열 선언

#### 검색 반복 및 값 담기

```java
			while(rs.next()) {
				zVO = new ZipCodeVO();
				zVO.setZipcode(rs.getInt("zipcode"));//숫자보다 문자열로 입력하는것이 직관적이다. 알아보기 편하다, 수정하기 좋다.
				zVO.setAddress(rs.getString("address"));
				v.add(zVO);
			}//end of while
			//List타입으로 선언된 v2는 copyInto메서드를 호출 할 수 없다.
			//부모가 가진 메서드를 재정의한것만 호출가능하기 때문이다. 타입이 부모이므로
			zVOS = new ZipCodeVO[v.size()];
			v.copyInto(zVOS);//copyinto메서드를 갖고있는건 Vector타입이다.
```

* rs.next\(\)로 커서를 조작하여 while문으로 반복한다.
* 2번 : 배열 생성
* 3번 : 배열에 int타입 zipcode를 set한다.
* 4번 : 배열에 String타입 address를 set한다.
* 5번 : Vector에 담긴 값들을 붙인다.
* 9번 : while문 밖에서 v.size\(\)로 방의 갯수가 정해는 객체배열을 생성한다.
* 10번 : Vector에 담긴 값들을 객체배열로 복사-붙여넣기 한다.

#### 초기화방법과 결과를 table에 올리기

```java
//질문 : 두 번 조회할 경우 앞에 조회결과가 남아있어요. 초기화하는 방법
			if(v.size() > 0) {
				while(dtm_zipcode.getRowCount()>0) {
					dtm_zipcode.removeRow(0);
				}
			}
			
			//조회된 결과를 DefaultTableModel에 매칭시키기
			for(int x=0;x<zVOS.length;x++) {
				Vector<Object> oneRow = new Vector<>();
				oneRow.add(0,zVOS[x].getZipcode());//0번째에 zipcode
				oneRow.add(1,zVOS[x].getAddress());//1번째에 address
				dtm_zipcode.addRow(oneRow);
			}
```

* 2번 : Vector에 값이 1개 이상 담겨져 있다면,
* 3번 : dtm 우편번호의 row가 0개 이상인 동안에는
* 4번 : dtm 우편번호의 row를 삭제한다.
* 9번 : 객체배열의 방 갯수보다 작은 경우에,
* 10번 : Object타입의 Vector타입인 oneRow 선언 및 생성
* 11번 : oneRow의 0번째에 객체배열\[x번방\]의 우편번호를 꺼낸다.
* 12번 : oneRow의 1번째에 객체배열\[x번방\]의 주소를 꺼낸다.
* 13번 : dtm에 oneRow를 파라미터로하는 Row를 add한다.

#### 예외처리 - catch

```java
		}catch(SQLException se) {
			System.out.println(se.toString());
			System.out.println("[[query]]=="+sql.toString());//toad에 돌려서 오타찾기
		}catch(Exception e) {
			System.out.println(e.toString());
		}//end of try-catch
		
	}//end of refreshData
```

* 1번 : sql문 Exception오류
* 2번 : String으로 출력
* 3번 : toda에 돌려서 오타를 찾을 수 있다.
* 4번 : 다른 Esception오류
* 5번 : String으로 출력

### 인터페이스 Override

```java
	@Override
	public void actionPerformed(ActionEvent e) {
		Object obj = e.getSource();
		if(obj == jbtn_search) {
			String user = jtf_search.getText();
			refreshData(user);
		}
		
	}//end of actionPerformed

	@Override
	public void mouseClicked(MouseEvent e) {
		if(e.getSource() == jtf_search) {
			jtf_search.setText("");
		}
		
	}

	@Override
	public void mouseEntered(MouseEvent e) {
		// TODO Auto-generated method stub
		
	}

	@Override
	public void mouseExited(MouseEvent e) {
		// TODO Auto-generated method stub
		
	}

	@Override
	public void mousePressed(MouseEvent e) {
		// TODO Auto-generated method stub
		
	}

	@Override
	public void mouseReleased(MouseEvent e) {
		// TODO Auto-generated method stub
		
	}//end of mouseReleased

}//end of class
```

* 사용할 메서드만 구현부분을 설정한다.
* 2-7번 : 조회버튼 이벤트가 감지되면 입력된 값을 String타입 user로 받아와 조회메서드를 호출한다.
* 12-15번 : 선언부 11번에서 설정한 TextField 값이 클릭시 초기화되는 이벤트

