---
description: 코드 분석
---

# DeptApp.java

```java
package oracle.db;

import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.MouseEvent;
import java.awt.event.MouseListener;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.Vector;

import javax.swing.JFrame;
import javax.swing.JMenu;
import javax.swing.JMenuBar;
import javax.swing.JMenuItem;
import javax.swing.JScrollPane;
import javax.swing.JTable;
import javax.swing.table.DefaultTableModel;

import book.ch5.DeptVO;
/* 생성자 안에서도 메소드 호출이 가능하다.
 * 변수 선언도 가능하다.
 * 내 안에 있는 메소드를 호출 하는 것이므로 인스턴스화는 필요하지 않다.
 * 생성자 앞에는 static이 없다.
 * non-static에서 non-static은 접근이 가능하고
 * static에서 non-static은 인스턴스화 해야 접근이 가능하다.
 */
public class DeptApp extends JFrame implements ActionListener, MouseListener {
	
	JMenuBar jmb    = new JMenuBar();	
	JMenu    jm_db  = new JMenu("오라클 연계");	
	JMenuItem jmi_con    = new JMenuItem("오라클연결");
	JMenuItem jmi_sel    = new JMenuItem("조회");
	JMenuItem jmi_ins    = new JMenuItem("등록");
	JMenuItem jmi_upd    = new JMenuItem("수정");
	JMenuItem jmi_del    = new JMenuItem("삭제");
	JMenuItem jmi_out    = new JMenuItem("나가기");
	//JTable : 양식, form을 그려주는 클래스이다.
	JTable    jt_dept    = null;//헤더를 처리할때 상속관계때문에 한번에 처리가 불가능하다.
	//scrollbar붙이려면 JTable하나 DefaultTableModel하나, JScollbar가 필요하다. = 속지들
	//JTable에 스크롤바를 붙여줄 속지
	JScrollPane jsp_dept = null;
	String cols[]        = {"부서번호","부서명","지역"};
	String data[][]      = new String[0][3];//[행][열]
	DefaultTableModel dtm = null;
	
	DBConnectionMgr dbMgr = new DBConnectionMgr();//한 메소드에만 적용할 호출이 아니므로 class의 아래에 인스턴스화한다.
	Connection      con   = null;
	PreparedStatement pstmt = null; //오라클 서버에 SELECT문을 가져가고 요청한 후 결과를 받아오는 역할 
	//조회버튼을 눌렀기때문에 PreparedStatment가 오라클에 가져가서 결과를 받아온다.
	ResultSet       rs    = null;//오라클에 사는 일꾼 옵티마이저

	public DeptApp() {//디폴트생성자
		initDisplay();
	}
	
	public void initDisplay() {
		dtm = new DefaultTableModel(data, cols);//인스턴스화 할때 new();에 ()생성자 자리이다.
		jt_dept = new JTable(dtm);
		jt_dept.addMouseListener(this);
		jsp_dept = new JScrollPane(jt_dept, JScrollPane.VERTICAL_SCROLLBAR_AS_NEEDED
				                          , JScrollPane.HORIZONTAL_SCROLLBAR_NEVER);;
		jmi_sel.addActionListener(this);//actionPerformed가 내 안에 있다.
		jm_db.add(jmi_sel);
		jm_db.add(jmi_ins);
		jm_db.add(jmi_upd);
		jm_db.add(jmi_del);
		jm_db.add(jmi_out);
		jmb.add(jm_db);
		this.add("Center",jsp_dept);
		this.setJMenuBar(jmb);		
		this.setSize(450, 320);
		this.setVisible(true);
	}

	public static void main(String[] args) {
		JFrame.setDefaultLookAndFeelDecorated(true);
		new DeptApp();
	}
	
	public void refreshData(String label) {
		//너 조회 버튼 누른거야?
		if("조회".contentEquals(label)) {//"조회"랑 버튼에서 가져온 label문자열이 같으면 실행.
			System.out.println("조회 버튼 클릭 성공");
			//java에서 select문 사용시, toad에서 단위테스트를 해본다.
			//String은 원본이 바뀌지 않으므로 +=로 대입을 해줘야 삽입이 된다. +=가 없으면 삽입되지않고 원본이 유지된다.
			String sql2  = "SELECT deptno, dname, loc FROM dept";//내 것을 갖고온다.
			       sql2 += " WHERE deptno >=?";//+=는 삽입 : where앞에 space가 없으면 deptWHERE로 인식하여 오류가 난다.
			       sql2 += "    OR loc LIKE ? || %";//개발자가 정할수없는, 사용자가 정하는 값 =? ,WHERE는 두번 올수 없다. and 나 or 사용
			       
			StringBuilder sql = new StringBuilder();
			sql.append("SELECT deptno, dname, loc FROM dept");
			sql.append(" WHERE deptno >=?");
			sql.append("    OR loc LIKE ?");
			       
			try {
				con = dbMgr.getConnection();//물리적으로 떨어져있는 거리를 연결
				pstmt = con.prepareStatement(sql.toString());//메모리에 로딩성공. 로딩 후에 읽어올 수 있다.==where92번
				pstmt.setInt(1, 30);//1=값의 개수, 30=값->값은 10-20-30 세번 돌지만. deptVO는 4칸 배열이라 4번돌려고 해서 오류가 발생한다.
		        pstmt.setString(2,  "N%");
				rs = pstmt.executeQuery();
		        DeptVO dvo = null; //인스턴스화1 : 선언
		        // DeptVO[] dvos = new DeptVO[4];//배열선언
		        DeptVO[] dvos = null;//몇건이 있는지 알수 없으므로 null로선언
		        Vector rv = new Vector<>();//오라클에게 건 수를 물어본 결과를 그때그때 기억한다.
		        int i = 0;
		        while(rs.next()) {//행(row)의 갯수만큼
		        	dvo = new DeptVO();//인스턴스화 2 : 클래스를 담는 주소번지 dvo 생성
		        	System.out.println(rs.getInt("deptno")
		        			              +", "+rs.getString("dname")
		        			              +", "+rs.getString("loc"));
		        	//dvo.setDeptno(rs.getInt("1"));//=87번 SELECT문의 첫번째 컬럼을 뜻한다. 직관적이지 않아 사용하지 않는다. ==101번
		        	dvo.setDeptno(rs.getInt("deptno"));//권장사항 == 컬럼명으로 표기
		        	dvo.setDname(rs.getString("dname"));
		        	dvo.setLoc(rs.getString("loc"));
		        	//dvos[i] = dvo;//dvos[0]이 첫 dvo값을 갖고 dvos[1]이 새 dvo값을 갖는다. - 반복
		        	rv.add(dvo);//오라클에서 얻은 정보를 그때그때 기억하는 문장
		        	i++;
		        }//end of while	
		        dvos = new DeptVO[rv.size()];//이제는 건 수를 결정할 수 있다. Vector의 size()를 호출하면 row수를 알 수 있으니까
		        rv.copyInto(dvos);//Vector에 담긴 정보를 객체배열에 똑같이 복사
		        
		        //화면에 출력하기
		        for(int j=0;j<dvos.length;j++) {
		        //for(int j=0;j<4;j++)
		        	DeptVO rdvo = dvos[j];//DeptVO=객체참조변수, 윈도우 바로가기파일
		        	System.out.println(rdvo.getDeptno()
		  	                            +", "+rdvo.getDname()
		  	                            +", "+rdvo.getLoc());
		        	Vector oneRow = new Vector<>();
					
					 oneRow.add(rdvo.getDeptno());
					 oneRow.add(rdvo.getDname());
					 oneRow.add(rdvo.getLoc());
					 
				    /*
				     * oneRow.add("10"); 
				     * oneRow.add("ACCOUNTTING"); 
				     * oneRow.add("NEW YORK");
				     */
		        	dtm.addRow(oneRow);
		        }
		        
			} catch (SQLException e2) {//end of try
				System.out.println("e2 SQLException : "+e2.toString());//try에서 오류발생시 실행=예외처리	
			}//end of catch
		}//end of if		
	}

	@Override
	public void actionPerformed(ActionEvent e) {
		String label = e.getActionCommand();//문자열 label가져오기		
		refreshData(label);
		
	}//end of actionPerformed

	@Override//mouse이벤트 모두를 정의해주어야 한다. 인터페이스가 반드시 구현해야하는 메소드 목록, 하나라도 없으면 문법에러가 발생한다.
	public void mouseClicked(MouseEvent e) {
		
	}

	@Override
	public void mouseEntered(MouseEvent e) {
		// TODO Auto-generated method stub
		
	}

	@Override
	public void mouseExited(MouseEvent e) {
		// TODO Auto-generated method stub
		
	}

	@Override//인터페이스가 반드시 구현해야하는 메소드 목록, 하나라도 없으면 문법에러가 발생한다.
	public void mousePressed(MouseEvent e) {//더블클릭
		if(e.getClickCount()==2) {
			System.out.println("더블 클릭 한거야");
		}//end of if		
	}

	@Override
	public void mouseReleased(MouseEvent e) {
		// TODO Auto-generated method stub
		
	}

}//end of class
```

#### StringBuilder

* 87-90번 : String
* 92-95번 : StringBuilder

#### Connection

* 98번  : con에 DBConnectionMgr 클래스 호출
* 99번  : 주소 인터페이스 pstmt에 con값을 sql로 가져온다. =PreparedStatment
* 102번: Resultset인터페이스 rs 에 가져온 값을 처리요청한다. =executeQuery 
* 108번: rs.next\(\)메서드로 row의 갯수만큼,  끝날때까지 while, 반복한다. 

#### Vector

* 100번 : int 30이 들어있는 방을 조회한다. \(10 &gt; 20 &gt; 30 : 3번 조회\) 
* 105번 : 객체배열을 사용하기위해  null로 선언한다. =row, 방의 갯수가 미
* 106번 : Vector를 사용하기위해 인스턴스화 한다.
* 118번 : Vector에 각기 다른 dvo를 기억시킨다. \(반복하는동안\)
* 121번 : 선언된 dvos배열을 Vector의 size를 인덱스값으로 하는 객체배열로 생성, 인스턴스화한다. - Vector가 기억한 dvo의 갯수만큼 인덱스를 갖는다.
* 122번 : Vector에 담긴 정보를 dvos배열에 복사한다.
* 125번 : 출력문구도 상수가 아닌, dvos.length만큼 반복한다.

#### SELECT

* 88번-95



