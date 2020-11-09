# MemberShip.java - 회원가입

```java
package design.book;

import java.awt.FlowLayout;
import java.awt.Font;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

import javax.swing.JButton;
import javax.swing.JComboBox;
import javax.swing.JDialog;
import javax.swing.JLabel;
import javax.swing.JOptionPane;
import javax.swing.JPanel;
import javax.swing.JPasswordField;
import javax.swing.JScrollPane;
import javax.swing.JTextField;

import desigin.test.ZipCodeSearch;

public class MemberShip extends JDialog implements ActionListener{
	//선언부
	ZipCodeSearch zcs = new ZipCodeSearch(this);
	
	JPanel  jp_south = new JPanel();
	JButton jbtn_ins = new JButton("등록");
	JButton jbtn_close = new JButton("닫기");
	
	JScrollPane jsp_line = new JScrollPane();
	
	JPanel     jp_center = new JPanel();
	JLabel     jlb_id    = new JLabel("아이디 : ");
	JTextField jtf_id    = new JTextField(10);
	JButton    jbtn_find = new JButton("중복검사");
	
	JLabel     jlb_pw  = new JLabel("비밀번호 : ");
	JLabel     jlb_pw2 = new JLabel("비밀번호 확인 : ");
	JPasswordField jpf_pw = new JPasswordField("12345678");
	JPasswordField jpf_pw2 = new JPasswordField("12345678");
	JButton    jbtn_pw = new JButton("확인");
	
	JLabel     jlb_name = new JLabel("이름 : ");
	JTextField jtf_name = new JTextField(20);
	JLabel     jlb_nick = new JLabel("닉네임 : ");
	JTextField jtf_nick = new JTextField(30);
	JLabel     jlb_gender = new JLabel("성별 : ");
	String[] genderList = {"남자", "여자"};
	JComboBox jcb_gender = new JComboBox(genderList);//고정된 상수값이므로 벡터사용하지 않는다.
	
	JLabel     jlb_zdo = new JLabel("우편번호 : ");
	JLabel     jlb_zdo2 = new JLabel("상세주소 : ");
	public JTextField jtf_zdo = new JTextField(6);
	public JTextField jtf_zdo2 = new JTextField(100);
	JButton    jbtn_zdo = new JButton("우편번호조회");
	
	//생성자
	public MemberShip() {
		initDisplay();
	}
	
	//화면처리부
	public void initDisplay() {
		
		jp_center.setLayout(null);
		jlb_id.setBounds(45, 20, 100, 20);//라벨이 입력창보다 왼쪽(앞)에 위치해야한다.
		jtf_id.setBounds(95, 20, 100, 20);
		jbtn_find.setBounds(200, 20, 90, 19);
		jp_center.add(jlb_id);
		jp_center.add(jtf_id);
		jp_center.add(jbtn_find);
		
		jlb_pw.setBounds(32, 50, 100, 20);
		jlb_pw2.setBounds(3, 70, 100, 20);
		jpf_pw.setBounds(95, 50, 100, 20);
		jpf_pw2.setBounds(95, 70, 100, 20);
		jbtn_pw.setBounds(200, 70, 60, 19);
		jp_center.add(jlb_pw);
		jp_center.add(jlb_pw2);
		jp_center.add(jpf_pw);
		jp_center.add(jpf_pw2);
		jp_center.add(jbtn_pw);
		
		jlb_name.setBounds(58, 100, 100, 20);
		jtf_name.setBounds(95, 100, 100, 20);
		jp_center.add(jlb_name);
		jp_center.add(jtf_name);
		jlb_nick.setBounds(45, 120, 100, 20);
		jtf_nick.setBounds(95, 120, 100, 20);
		jp_center.add(jlb_nick);
		jp_center.add(jtf_nick);
		jlb_gender.setBounds(200, 100, 100, 20);
		jcb_gender.setBounds(240, 100, 58, 19);
		jcb_gender.setFont(new Font("굴림",1,12));
		jp_center.add(jlb_gender);
		jp_center.add(jcb_gender);
		
		jlb_zdo.setBounds(32, 150, 100, 20);
		jlb_zdo2.setBounds(32, 170, 100, 20);
		jtf_zdo.setBounds(95, 150, 100, 20);
		jtf_zdo2.setBounds(95, 170, 220, 20);
		jbtn_zdo.setBounds(200, 150, 114, 19);
		jp_center.add(jlb_zdo);
		jp_center.add(jlb_zdo2);
		jp_center.add(jtf_zdo);
		jp_center.add(jtf_zdo2);
		jp_center.add(jbtn_zdo);
		
		jsp_line = new JScrollPane(jp_center);
		
		jbtn_zdo.addActionListener(this);
		jbtn_pw.addActionListener(this);
		jbtn_close.addActionListener(this);
		jbtn_ins.addActionListener(this);
		
		jp_south.setLayout(new FlowLayout(FlowLayout.RIGHT));
		jp_south.add(jbtn_ins);
		jp_south.add(jbtn_close);
		
		
		this.add("Center", jsp_line);
		this.add("South", jp_south);
		this.setTitle("회원가입");
		this.setSize(350, 600);
		this.setVisible(true);
	}
	
	public static void main(String[] args) {
		new MemberShip();		
	}

	@Override
	public void actionPerformed(ActionEvent e) {
		Object obj = e.getSource();
		
		if(obj==jbtn_close) {
			dispose();
		}
		if(obj==jbtn_ins) {
			if("".equals(jtf_id.getText())) {
				JOptionPane.showMessageDialog(this, "아이디를 입력하세요."
											   , "INFO"
											   , JOptionPane.INFORMATION_MESSAGE);
				return;//탈출
			}
			else if("".equals(jpf_pw.getText())) {
				JOptionPane.showMessageDialog(this, "비밀번호를 입력하세요."
						   , "INFO"
						   , JOptionPane.INFORMATION_MESSAGE);
				return;//탈출
		    }
		}//end of jbtn_ins if
		
		if(obj==jbtn_pw) {
			if(jpf_pw.equals(jpf_pw2)) {
				JOptionPane.showMessageDialog(this, "확인되었습니다."
						   , "INFO"
						   , JOptionPane.INFORMATION_MESSAGE);
				return;
			}
			else {
				JOptionPane.showMessageDialog(this, "비밀번호가 다릅니다. 확인해주세요."
													, "INFO"
													, JOptionPane.INFORMATION_MESSAGE);
				return;
			}
		}//end of jbtn_pw if
		
		if(obj==jbtn_zdo) {
			zcs.setLocation(800, 250);
			zcs.initDisplay();
		}//end of jbtn_zdo if
	}//end of Override
}//end of class
```

