# NickNameList.java +

### + import

```java
package desigin.test;

import java.awt.BorderLayout;
import java.awt.Color;
import java.awt.Font;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

import javax.swing.Icon;
import javax.swing.ImageIcon;
import javax.swing.JButton;
import javax.swing.JMenu;
import javax.swing.JMenuBar;
import javax.swing.JMenuItem;
import javax.swing.ListSelectionModel;
import javax.swing.table.JTableHeader;
```

### +선언부

```java
//디폴트 생성자는 생략가능 - 파라미터가 없으므로 JVM이 대신 해줄수 있기때문이다.
public class NickNameList extends JFrame implements MouseListener, ActionListener{
	NickNameList nnl = null;
	NickNameListSub nnls = new NickNameListSub(this);

	static NickNameList nnList = null;

	DefaultTableModel dtm = new DefaultTableModel(data, cols);
	
	JTable jtb = new JTable(dtm);
	JTableHeader jth = jtb.getTableHeader();

	String imgPath = "C:\\workspace_java\\dev_java\\src\\desigin\\test\\";
	//절대경로, 상대경로
	
	Font   f        = new Font("굴림체",Font.BOLD,20);//API 참고
	JPanel jp_north = new JPanel();
	//BorderLayout-center : 꽉 찬다, flowLayout는 지정한다. 
	//생성자 파라미터로 컬럼의 크기를 지정하여 TextField의 사이즈를 결정해주어야 한다. 
	//new TestField(25)
	JTextField jtf_search  = new JTextField();
	JButton    jbtn_search = new JButton("검색");
	//선언과 생성을 분리할 것인지 한번에 처리할 것인지
	JMenuBar jmb      = new JMenuBar();	
	JMenu    jm_edit  = new JMenu("Edit");
	JMenuItem jmi_ins = new JMenuItem("입력", new ImageIcon(imgPath+"new.gif"));
	JMenuItem jmi_upd = new JMenuItem("수정", new ImageIcon(imgPath+"update.gif"));
	JMenuItem jmi_del = new JMenuItem("삭제", new ImageIcon(imgPath+"delete.gif"));
	JMenuItem jmi_search = new JMenuItem("조회", new ImageIcon(imgPath+"detail.gif"));
	
	Icon icon = new ImageIcon(imgPath+"delete.gif");
	JMenuItem jmi_exit   = new JMenuItem("나가기",icon);
```

* Table의 Header\(컬럼명\)을 불러올수 있는 getTableHeader함수를 사용하기위한 선언 추가
* 14번 : 이미지를 불러올 경로 선언, 경로의 마감이 \\로 끝나야한다.
* 여러 이미지를 불러올거면 경로가 반복되야 한다. -&gt; for문이 필요하다.
* 16번 : 지정 Font를 멤버변수로 선언, API에서 Font는 static이 붙어있다. - 호출시 : Font.클래스명 
* 22번 : JTextField는 기본값이 없어 값을 넣어야 크기를 만들 수 있다. -&gt; 크기를 정해주자\(화면구현\)
* 26-34번 : 메뉴는 멤버변수로 선언, 초기화한다. - actionPerformed함수에서도 그 주소번지를 사용해야 하기 때문이다.
* 26-29번 : 이미지를 넣는 방법 1 ex\)JButton - **JButton jbtn1 = new 클래스\("버튼이름", new ImageIcon\(imgPath+"이미지명.속성"\)\);**
* 31-32번 : 이미지를 넣는 방법 2 - **Icon icon = new ImageIcon\(imgPath+"이미지명.속성"\);** - **JButton jbtn2 = new 클래스명\("버튼이름", icon\);**

### +화면구현 메서드

```java
	public void initDisplay() {//this는 생략가능
		
		jp_north.setLayout(new BorderLayout());
		
		jtf_search.setFont(f);//setFont:void = 반환할게 없으므로 void, 폰트를 set한다.
		//jtf_search.setFont(new Font("",,));
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
	}
```

* 기본 레이아웃은 FlowLayout이다.
* 3번 : TextField에 크기를 넣기위애서 TextField가 올라갈 속지Layout을 BorderLayout으로 변경한다.
* 5번 : jtf\_search입력창의 폰트를 정해준다. setFont함수는 변형시키는 역할로 반환타입이 void이다.
* 6번 : 새 Font를 지역변수 개념으로 지정해주는 것이므로 다른 함수에서 꺼내쓸수없다.
* 10-15번 : 메뉴 add
* 17-22번 : 이벤트 매칭
* 27번 : Table의 row의 높이를 25로 초기화
* 30번 : jtb를 선택할때 하나만 선택할 수 있도록 해준다. - **객체.setSelectionMode\(ListSelectionModel.SINGLE\_SELECTION\);**

### +메서드로 기능 꺼내기

```java
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
	//전체 조회 혹은 조건검색 구현 할때 똑같은이벤트가 필요하다.
	public void refreshData() {
		NickNameListSub nnls = new NickNameListSub();
	}
```

* 다른 이벤트 발생시에도 Sub창을 이용해야 한다. -&gt; 메서드로 꺼내 재사용해야한다.
* 인스턴스화 처리 메서드롤 따로 만들어 추가했다.

### +이벤트 처리

```java
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
```

* 메뉴에 대한 이벤트를 추가

