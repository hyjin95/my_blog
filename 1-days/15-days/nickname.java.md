# NickName.java

### 사용된 낱말카드

getter, setter, 생성자, 인스턴스화 분리, 배열, 이벤트처리, UI\(레이아웃\), 추상클래스, 인터페이스, override, 파라미터, 추상메서드, 일반메서드, 변수, 선언과 생성위

### NickNameList.java

```java
package desigin.test;

import java.awt.event.MouseEvent;
import java.awt.event.MouseListener;

import javax.swing.JFrame;
import javax.swing.JOptionPane;
import javax.swing.JScrollPane;
import javax.swing.JTable;
import javax.swing.table.DefaultTableModel;

//디폴트 생성자는 생략가능 - 파라미터가 없으므로 JVM이 대신 해줄수 있기때문이다.
public class NickNameList extends JFrame implements MouseListener{
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
	JScrollPane jsp = new JScrollPane(jtb
									 ,JScrollPane.VERTICAL_SCROLLBAR_AS_NEEDED
									 ,JScrollPane.HORIZONTAL_SCROLLBAR_AS_NEEDED);
	
	public NickNameList() {}
	
	public void initDisplay() {//this는 생략가능
		jtb.addMouseListener(this);
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
				NickNameListSub nnls = new NickNameListSub();
				//이벤트가 일어낫을때 initDisplay호출이므로->true여도되지만 전변에 있을경우에는 반드시 false해주어야한다.
			}			
		}//end of if			
	}

	@Override
	public void mouseReleased(MouseEvent e) {
		// TODO Auto-generated method stub
		}
}//end of class
//Sub호출 -> 인스턴스화 -> 지변인지 전변인지(위치가 달라진다)
//여기서는 77번 else에 해야하는데 그러면 지변이된다.
```

### NickNameListSub.java

```java
package desigin.test;

import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

import javax.swing.JButton;
import javax.swing.JDialog;
import javax.swing.JLabel;
import javax.swing.JPanel;
import javax.swing.JScrollPane;
import javax.swing.JTextField;

public class NickNameListSub extends JDialog{
	NickNameList nnl     = null;
	JScrollPane  jspLine = null;//jp_center를 얹어야 하기때문에 생성을 분리해야한다.
	//현재 중앙에 JScrollPane를 사용한 상태이고 그 이유는 테두리선을 활용하기 위함이다.
	//그 위에 새로운 속지를 얹어서 추가적인 화면 컴포넌트를 배치할 수 있다.
	//이때 Layout대신 일일이 좌표값을 활용하여 배치해본다.
	JPanel      jp_center  = new JPanel();
	JLabel      jlb_gender = new JLabel("성별");
	JTextField  jtf_gender = new JTextField();
	
	JPanel      jp_south   = new JPanel();
	JButton     jbtn_save  = new JButton("저장");
	JButton     jbtn_close = new JButton("닫기");
	
	public NickNameListSub() {
		initDisplay();
	}
	public void update() {
		System.out.println("수정 메서드 호출 성공");
	}
		
	public NickNameListSub(NickNameList nnl) {
		this.nnl = nnl;
	}
	
	public void initDisplay() {
		//JDialog의 디폴트 레이아웃은 BorderLayout이다.
		//직접 좌표값을 활용해서 배치할때에는 레이아웃이 필요없다.
		jp_center.setLayout(null);//레이아웃 뭉개기
		jlb_gender.setBounds(20, 20, 100, 20);//라벨이 입력창보다 왼쪽(앞)에 위치해야한다.
		jtf_gender.setBounds(70, 20, 100, 20);
		jp_center.add(jlb_gender);
		jp_center.add(jtf_gender);
		jspLine = new JScrollPane(jp_center);//위에서 라벨과 입력창의 위치를 결정한 후에, 사용되기 전에 생성되어야 한다.
		
		jbtn_save.addActionListener(new ActionListener() {
			@Override
			public void actionPerformed(ActionEvent ae) {
				update();
			}
		});
		jbtn_close.addActionListener(new ActionListener() {
			@Override
			public void actionPerformed(ActionEvent ae) {
				System.exit(0);
			}
		});
		this.add("South", jp_south);//JPanel속지를 화면의 남쪽에 붙이기
		jp_south.add(jbtn_close);//JPanel에 버튼 붙이기
		this.add("Center", jspLine);//테두리 선
		this.setTitle("상세보기");
		this.setSize(300, 200);
		this.setVisible(true);
		//전변에 인스턴스화를 하면 true면 누르지않아도보인다. 하지만 지금은 이벤트가 일어낫을때 호출되므로 지금은 true여도된다.
	}//end of initDisplay
	
	////////////////////[setter and getter[///////////////////
	public void getGender(String gender) {
		jtf_gender.setText(gender);
	}	
	public void getGender() {//get이지만 void인 이유는 getText하면 어차피 string으로 꺼내므로
		jtf_gender.getText();//가져온값 member변수에 담기
		MemberVO mVO = new MemberVO(null, jtf_gender.getText());//생성자의 파라미터를 사용한 초기화
		//MemberVO mVO = new MemberVO();-메서드를 이용한 초기화
		//mVO.setGender(jtf_gender.getText());-메서드를 이용한 초기화2
	}	
	////////////////////[setter and getter[///////////////////	
}//end of class
//actionListener은 인터페이스로 단독으로 인스턴스화할 수 없다.
//new actionListener 불가능
//단, 클래스 안에 actionPerformed(구현체 클래스)가 있으면 그 안에서 사용할 수 있다.
//actionListener a = new NickNamdDetail(); -implememts한 경우 액션리스너가 더 크므로 왼쪽항에
//actionListener a = new actionListener();은 불가능하다. 오른쪽항에는 인터페이스가 아닌 클래스가 와야 에러가 나지않는다.
//intitDisplay에서 actionPerformed메서드를 달았기때문에 가능한것이다.addActionListener(new ActionListener())=메서드(클래스);
```

### MemberVO.java

컬럼명들이 갖고있는 get으로 값을 꺼내오고 set으로 저장하는 클래스

```java
package desigin.test;

public class MemberVO {
	private String nickName = null;
	private String gender   = null;
	
	public MemberVO() {}
	
	public MemberVO(String nickName, String gender) {//갯수만큼 다 써주어야함
		this.nickName = nickName;
		this.gender = gender;
	}
	
	public String getNickName() {
		return nickName;
	}
	public void setNickName(String nickName) {
		this.nickName = nickName;
	}
	public String getGender() {
		return gender;
	}
	public void setGender(String gender) {
		this.gender = gender;
	}
}
//우클릭 - Source - Generate getter and setter - select all - last member - finish
```

