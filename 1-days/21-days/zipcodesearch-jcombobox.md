# ZipcodeSearch - JcomboBox, FocusListener



## ZipCodeSearch.java

### import

```java
import java.awt.BorderLayout;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.FocusEvent;
import java.awt.event.FocusListener;
import java.awt.event.ItemEvent;
import java.awt.event.ItemListener;
import java.awt.event.MouseEvent;
import java.awt.event.MouseListener;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.List;
import java.util.Vector;

import javax.swing.JButton;
import javax.swing.JComboBox;
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
public class ZipCodeSearch extends JFrame implements ItemListener,FocusListener,ActionListener, MouseListener {
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
	
	//1번 생성자
	String zdos[] = {"전체","서울", "경기", "강원"};//String 타입 배열생성
	JComboBox jcb_zdo = new JComboBox(zdos);
	//2번생성자
	String zdos2[] = {"전체","부산", "전남", "대구"};//String 타입 배열생성
	Vector<String> vzdos = new Vector<>();//현재 vzdos.size()는 0
	JComboBox jcb_zdo2 = null;//선언과 생성을 분리해야한다.
	/* String배열에 있는 정보들을 굳이 벡터에 담아야한다.
	 * 생성자 선택을 Vector타입으로 결정했기 때문이다.
	 * 벡터 자체는 값은 가지고 있지 않은 상태이다.
	 * 값은 1차 배열안에 초기화되어있다.
	 * 이것을 벡터에 재 초기화해야한다.
	 * 그 후에 콤보박스를 생성하고 화면에 장착해야 값이 담긴다.
	 */

	String cols[]   = {"우편번호", "주소"};
	String data[][] = new String[0][2];//0번째, 컬럼 2개
	DefaultTableModel dtm_zipcode = new DefaultTableModel(data,cols);//데이터 얹기
	JTable jtb_zipcode = new JTable(dtm_zipcode);//양식제공
	JScrollPane jsp_zipcode = new JScrollPane(jtb_zipcode
											  , JScrollPane.VERTICAL_SCROLLBAR_AS_NEEDED
											  , JScrollPane.HORIZONTAL_SCROLLBAR_AS_NEEDED);//스크롤바 제공
	String zdos3[] = null;
	String zdo = null;
```

### 생성자

```java
public ZipCodeSearch() {
		zdos3 = getZdoList();//화면구성보다 먼저 호출되어야 한다.버튼정의이므로
		//DB를 다녀온 후에 화면을 출력해야한다. 지역버튼은 그래야 정의되므로
		//getZdoList메서드의 반환타입은 배열이다.
		initDisplay();
	}
```

### 화면구현부

```java
public void initDisplay() {
//		//2번 생성자
//		for(int i=0;i<zdos.length;i++) {
//			vzdos.add(zdos2[i]);
//		}
//		jcb_zdo2 = new JComboBox(vzdos);
//		vzdos.copyInto(zdos);
//		//2번 생성자 끝
		
		jcb_zdo2 = new JComboBox(zdos3);
		
		jtb_zipcode.requestFocus();
		jbtn_search.addActionListener(this);
		jtf_search.addFocusListener(this);
		jtf_search.addMouseListener(this);
		jcb_zdo2.addItemListener(this);
		
		jp_north.setLayout(new BorderLayout());
		jp_north.add("Center", jtf_search);
		jp_north.add("East", jbtn_search);
		jp_north.add("West", jcb_zdo2);	
		
		this.add("Center", jsp_zipcode);
		this.add("North", jp_north);
		this.setTitle("우편번호 검색");
		this.setSize(430, 400);
		this.setVisible(true);
	}//end of initDisplay	
```

### 메인메서드

```java
	public static void main(String[] args) {
		ZipCodeSearch zcs = new ZipCodeSearch();		
	}//end of main
```

### 주소 DB 연결 메서드

```java
public String[] getZdoList() {
		//원격에 있는 오라클 서버에 접속하기위해 DBConnectionMgr객체 생성
		DBConnectionMgr dbMgr  = DBConnectionMgr.getInstance();
		//물리적으로 떨어져있는 서버에 연결통로 확보
		con = dbMgr.getConnection();
		String[] zdos = null;
		//DB연동하기-쿼리문준비
		StringBuilder sql = new StringBuilder();
		sql.append("SELECT '전체' zdo FROM dual");
		sql.append(" UNION ALL");
		sql.append(" SELECT zdo");
		sql.append(" FROM (SELECT distinct(zdo) zdo");
		sql.append("         FROM zipcode_t");
		sql.append("        ORDER BY zdo asc)");
		
		try {//인터넷이 끊기는 경우 에러 발생
			//전령객체 메모리에 로딩
			pstmt = con.prepareStatement(sql.toString());
			//전령객체 로딩시 파라미터로 SELECT문을 전달했다.
			//SELECT문을 처리해주세요.=rs
			//I,U,D의 경우에는 executeUpdate
			rs = pstmt.executeQuery();
			String imsi = null;
			Vector<String> v = new Vector<>();
			while(rs.next()) {//값 꺼내기 반복
				imsi = rs.getString("zdo");		
				v.add(imsi);
			}
			zdos = new String[v.size()];
			v.copyInto(zdos);
		} catch (Exception e) {
			System.out.println("Exception : "+e.toString());
		}		
		return zdos;
	}//end of getZdoList
```

### 조회메서드에 주소 추가

```java
public void refreshData(String user, String dong) {
		System.out.println("zdo : "+zdo+", dong : "+dong);
		DBConnectionMgr dbMgr  = DBConnectionMgr.getInstance();
		con = dbMgr.getConnection();
		StringBuilder sql = new StringBuilder();
		sql.append("SELECT zipcode, address");
		sql.append(" FROM zipcode_t");
		sql.append(" WHERE 1=1");
		if(zdo!=null && zdo.length()>0) {//비어있지 않고, 하나 이상 값이 있으면
			sql.append(" AND zdo=?");//WHERE절 다음에 붙으므로 AND절을 사용
		}
		if(dong!=null) {
			sql.append(" AND dong LIKE '%'||?||'%'");//전체검색
		}
		//물음표의 갯수가 달라지므로 변수처리한다.
		//물음표 자리는 배열이 아니므로 1부터이다.
		int i = 1;
		try {
			//파라미터로 넘어온 select문을 스캔 -> ?갯수를 파악한다.
			pstmt = con.prepareStatement(sql.toString());
			//위에서 물음표 자리에 들어갈 값을 파라미터로 받아서 설정한다.1=?갯수
			if(zdo!=null && zdo.length()>0) {//비어있지 않고, 하나 이상 값이 있으면
				sql.append(" AND zdo=?");
				pstmt.setString(i++, zdo);//콤보박스에서 가져온 값
			}
			if(dong!=null && dong.length()>0) {
				pstmt.setString(i++, dong);//텍스트 필드에서 가져온 값
			}
			rs = pstmt.executeQuery();
			//내 안에 있는 타입을 꺽쇠<>안에 직접 써주면 타입체크를 별도로 하지 않는다. = 제네릭
			//선언부에는 반드시 써야하고 생성부에서는 생략가능하다.
			//그러나 다이아몬드 연산자는 작성해준다 = 오른쪽항
			//Vector v = new Vector();
			Vector<ZipCodeVO> v = new Vector<>();//권장사항
			//List v2 = new Vector(); //같다, 선언부에 인터페이스 하지만 List는 copyinto를 쓸 수 없으므로 사용하지 않는다.
			ZipCodeVO zVOS[] = null;
			ZipCodeVO zVO = null;
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
			
		}catch(SQLException se) {
			System.out.println(se.toString());
			System.out.println("[[query]]=="+sql.toString());//toad에 돌려서 오타찾기
		}catch(Exception e) {
			System.out.println(e.toString());
		}//end of try-catch
		
	}
```

### Override

```java
	@Override
	public void actionPerformed(ActionEvent e) {
		Object obj = e.getSource();
		if(obj == jbtn_search) {
			String user = jtf_search.getText();
			refreshData(zdo, user);//지역을 추가햇으므로 지역도 검색에 포함시켜보자, 지역은 고르지 않을 수도 있다.
		}
		
	}//end of actionPerformed

	@Override
	public void focusGained(FocusEvent arg0) {
		// TODO Auto-generated method stub
		
	}

	@Override
	public void focusLost(FocusEvent arg0) {
		// TODO Auto-generated method stub
		
	}//end of focus	
	
	@Override
	public void itemStateChanged(ItemEvent e) {
		Object obj = e.getSource();
		if(obj == jcb_zdo2) {
			if(e.getStateChange() == ItemEvent.SELECTED) {
				zdo = zdos3[jcb_zdo2.getSelectedIndex()];
			}
		}
		
	}//end of if

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
		
	}
```

## 테스트 - ZipCodeDaoSimulation.java

```java
package desigin.test;

public class ZipCodeDaoSimulation {
	
	public static void main(String[] args) {
		
		ZipCodeSearch zc = new ZipCodeSearch();
		String[] zdos = zc.getZdoList();
		for(String s:zdos) {
			System.out.println(s);
		}
	}		
}
```

