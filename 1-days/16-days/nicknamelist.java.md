# NickNameList.java +

```java
package desigin.test;

import java.awt.BorderLayout;
import java.awt.Color;
import java.awt.Font;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.MouseEvent;
import java.awt.event.MouseListener;

import javax.swing.Icon;
import javax.swing.ImageIcon;
import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JMenu;
import javax.swing.JMenuBar;
import javax.swing.JMenuItem;
import javax.swing.JOptionPane;
import javax.swing.JPanel;
import javax.swing.JScrollPane;
import javax.swing.JTable;
import javax.swing.JTextField;
import javax.swing.ListSelectionModel;
import javax.swing.table.DefaultTableModel;
import javax.swing.table.JTableHeader;

//디폴트 생성자는 생략가능 - 파라미터가 없으므로 JVM이 대신 해줄수 있기때문이다.
public class NickNameList extends JFrame implements MouseListener, ActionListener{
	NickNameList nnl = null;
	NickNameListSub nnls = new NickNameListSub(this);
	//table을 바로 보여주느냐 조회하면 보여주느냐, 선언과생성을 같이하느냐 따로하느냐
	static NickNameList nnList = null;//인스턴스화를 하는 새로운 방법-33번
	String cols[]   = {"대화명", "성별"};//선언과 생성 같이
	String data[][] = {
			{"test", "남"}
		   ,{"apple", "여"}
		   ,{"tomato", "여"}
	};//end of data
	
	DefaultTableModel dtm = new DefaultTableModel(data, cols);
	//테이블 생성시 데이터셋에 대한 헤더 초기화 부분이 필요하다. - btm이 이때 사용된다.
	JTable jtb = new JTable(dtm);
	JTableHeader jth = jtb.getTableHeader();
	JScrollPane jsp = new JScrollPane(jtb
									 ,JScrollPane.VERTICAL_SCROLLBAR_AS_NEEDED
									 ,JScrollPane.HORIZONTAL_SCROLLBAR_AS_NEEDED);
	//이미지를 불러오는 경로 = 반복
	String imgPath = "C:\\workspace_java\\dev_java\\src\\desigin\\test\\";//절대경로, 상대경로
	
	Font       f           = new Font("굴림체",Font.BOLD,20);//API Font참고, static이라서 Font.클래스이름 하면 된다.
	JPanel     jp_north    = new JPanel();
	//기본값이 없어서 값을 넣어줘야 크기를 볼수있다 -> 크기를 정해주자 
	//BorderLayout-center : 꽉 찬다, flowLayout는 지정한다. 
	//생성자 파라미터로 컬럼의 크기를 지정하여 TextField의 사이즈를 결정해주어야 한다. new TestField(25)
	JTextField jtf_search  = new JTextField();
	JButton    jbtn_search = new JButton("검색");
	
	//메뉴 - 전역변수로 한다. actionPerformed에서도 그 주소번지를 사용해야 하므로
	//선언과 생성을 분리할 것인지 한번에 처리할 것인지
	JMenuBar jmb      = new JMenuBar();	
	JMenu    jm_edit  = new JMenu("Edit");
	JMenuItem jmi_ins = new JMenuItem("입력", new ImageIcon(imgPath+"new.gif"));
	JMenuItem jmi_upd = new JMenuItem("수정", new ImageIcon(imgPath+"update.gif"));
	JMenuItem jmi_del = new JMenuItem("삭제", new ImageIcon(imgPath+"delete.gif"));
	JMenuItem jmi_search = new JMenuItem("조회", new ImageIcon(imgPath+"detail.gif"));
	
	Icon icon = new ImageIcon(imgPath+"delete.gif");
	JMenuItem jmi_exit   = new JMenuItem("나가기",icon);
	
	public NickNameList() {}
	
	public void initDisplay() {//this는 생략가능
		
		jp_north.setLayout(new BorderLayout());//기본flowlayout에서 크기를 만들어주기위해 borderlayout으로 set해줌
		jtf_search.setFont(f);//setFont:void = 반환할게 없으므로 void, 폰트를 set한다.
		//jtf_search.setFont(new Font("",,));//해도되지만 외부에서는 사용할수 없다.
		jp_north.add("Center", jtf_search);
		jp_north.add("East", jbtn_search);
				
		jm_edit.add(jmi_ins);
		jm_edit.add(jmi_upd);
		jm_edit.add(jmi_del);
		jm_edit.add(jmi_search);
		jm_edit.add(jmi_exit);
		jmb.add(jm_edit);
		
		jtb.addMouseListener(this);
		jmi_ins.addActionListener(nnList);
		jmi_upd.addActionListener(nnList);
		jmi_del.addActionListener(nnList);
		jmi_search.addActionListener(nnList);
		jmi_exit.addActionListener(nnList);
		
		jth.setBackground(new Color(70,95,217));
		jth.setForeground(new Color(255,255,255));
		jth.setFont(f);
		jtb.setRowHeight(25);
		jtb.setGridColor(Color.DARK_GRAY);
		jtb.setSelectionBackground(new Color(255,220,239));
		jtb.setSelectionMode(ListSelectionModel.SINGLE_SELECTION);
		nnList.setJMenuBar(jmb);
		nnList.add("North", jp_north);
		nnList.add("Center", jsp);
		nnList.setTitle("단톡명단");
		nnList.setSize(450, 500);
		nnList.setVisible(true);
	}

	public static void main(String[] args) {
		nnList = new NickNameList();
		nnList.initDisplay();//한번만 호출		
	}//end of main

	@Override
	public void mouseClicked(MouseEvent e) {
		// TODO Auto-generated method stub
		
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
		if(e.getClickCount()==2) {
			int index[] = jtb.getSelectedRows();			
			if(index.length == 0) {//아무것도 선택하지 않았으면
				JOptionPane.showMessageDialog(this, "조회할 데이터를 선택하시오.");			
				return;
			}//end of if
			else if(index.length >= 2) {
				JOptionPane.showMessageDialog(this, "데이터를 한 건만 선택하시오.");
				return;
			}
			else {
				String nickName = (String)dtm.getValueAt(index[0], 0);
				System.out.println("nickName : "+nickName);
				refreshData();
			}			
		}//end of if			
	}

	@Override
	public void mouseReleased(MouseEvent e) {
		// TODO Auto-generated method stub
		}
	//전체 조회 혹은 조건검색 구현 할때 똑같은이벤트가 필요하다 = 메서드로 꺼낸다.
	//재사용할 수 있도록 메서드로 처리한다.
	public void refreshData() {
		NickNameListSub nnls = new NickNameListSub();
		//이벤트가 일어낫을때 initDisplay호출이 -> true여도되지만 전변에 있을경우에는 반드시 false해주어야한다.
	}
	@Override
	public void actionPerformed(ActionEvent ae) {
		Object obj = ae.getSource();
		//너 입력할거니?
		if(obj == jmi_ins) {
			System.out.println("입력호출");
		}		
		//수정을 원해?
		else if(obj == jmi_upd) {
			System.out.println("수정호출");			
		}
		//삭제해줄까?
		else if(obj == jmi_del) {
			System.out.println("삭제호출");			
		}
		//전체 조회하고싶어
		else if(obj==jmi_search) {
			refreshData();			
		}
		//나가기
		else if(obj==jmi_exit) {
			System.exit(0);
		}		
	}
}//end of class
//Sub호출 -> 인스턴스화 -> 지변인지 전변인지(위치가 달라진다)
//여기서는 77번 else에 해야하는데 그러면 지변이된다.
```

