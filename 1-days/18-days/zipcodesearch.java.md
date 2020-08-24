# ZipCodeSearch.java - 우편번호 조회

### +NickNameListSub.java

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

## ZipCodeSearch.java

#### 우편번호 검색시 자료보여주기, 참고 : ZipCodeVO.java

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
	
	//조회메서드
	public void refreshData(String dong) {
		DBConnectionMgr dbMgr  = DBConnectionMgr.getInstance();
		con = dbMgr.getConnection();
		StringBuilder sql = new StringBuilder();
		sql.append("SELECT zipcode, address");
		sql.append(" FROM zipcode_t");
		sql.append(" WHERE 1=1");
		if(dong!=null) {
			sql.append(" AND dong LIKE '%'||?||'%'");//전체검색
		}
		try {
			//파라미터로 넘어온 select문을 스캔 -> ?갯수를 파악한다.
			pstmt = con.prepareStatement(sql.toString());
			//위에서 물음표 자리에 들어갈 값을 파라미터로 받아서 설정한다.1=?갯수
			pstmt.setString(1, dong);
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

