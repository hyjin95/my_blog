# Login

#### 기존에 만들어둔 화면클래스를 복사해와 사용한다.

```java
package net.tomato_step2;
import java.awt.Font;
import java.awt.Graphics;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

import javax.swing.ImageIcon;
import javax.swing.JButton;
//이미지 넣어서 만들기
import javax.swing.JFrame;
import javax.swing.JLabel;
import javax.swing.JOptionPane;
import javax.swing.JPanel;
import javax.swing.JPasswordField;
import javax.swing.JTextField;

public class Login extends JFrame implements ActionListener {
	//선언부
	String imgPath = "C:\\workspace_java\\dev_java\\src\\images\\";
	ImageIcon iicon_m = new ImageIcon(imgPath+"mains.png");
	
	JLabel jlb_id = new JLabel("아이디");//"아이디"가 변경될 일이 없으므로 멤버변수로 선언,생성한다.
	JLabel jlb_pw = new JLabel("패스워드");
	
	JTextField jtf_id = new JTextField("test");
	JTextField jpf_pw = new JTextField("123");
	
	JPanel jp_login = new JPanel();	
	
	JButton jbtn_login = new JButton(new ImageIcon(imgPath+"login.png"));
	JButton jbtn_join = new JButton(new ImageIcon(imgPath+"new.png"));
	
	Font f = new Font("나눔고딕",Font.BOLD,17);
	
	BandClient bc = null;
	BendMemberShip bmb = null;	
	
	String nickName = null;
	
	class MyPicture extends JPanel {
		public void paintComponent(Graphics g) {//paintComponent그리기 클래스 파라미터는 반드시 Graphics g
			g.drawImage(iicon_m.getImage(), 0, 0, null);
			setOpaque(false);//옵션값 죽이기
			super.paintComponent(g);//super=부모
		}
	}
	
	//생성자
	public Login() {
		
	}
	
	//화면처리부
	public void initDisplay() {//setBound로 id창+tf, pw창+tf2개
		
		jbtn_login.addActionListener(this);
		jbtn_join.addActionListener(this);
		
		setContentPane(new MyPicture());
		this.setLayout(null);
		jlb_id.setBounds(45, 200, 80, 40);
		jlb_id.setFont(f);
		this.add(jlb_id);		
		jtf_id.setBounds(110, 200, 185, 40);
		jtf_id.setFont(f);
		this.add(jtf_id);
		
		jlb_pw.setBounds(30, 245, 80, 40);
		jlb_pw.setFont(f);
		this.add(jlb_pw);		
		jpf_pw.setBounds(110, 245, 185, 40);
		jpf_pw.setFont(f);
		this.add(jpf_pw);
		
		jbtn_login.setBounds(45, 295, 120, 40);
		jbtn_login.setFont(f);
		this.add(jbtn_login);
		
		jbtn_join.setBounds(175, 295, 120, 40);
		jbtn_join.setFont(f);
		this.add(jbtn_join);
		
		
		this.setTitle("도서관리 시스템");
		this.setSize(350, 600);
		this.setLocation(800, 250);//띄워질 좌표 설정
		this.setVisible(true);		
	}
	//메인메서드
	public static void main(String[] args) {
		Login lf = new Login();
		lf.initDisplay();
	}

	@Override
	public void actionPerformed(ActionEvent e) {
		Object obj = e.getSource();//이벤트가 감지된 주소번지 달기
		
		if(obj==jbtn_login) {
			if("".equals(jtf_id.getText())) {
				JOptionPane.showMessageDialog(this, "아이디를 확인하세요."
											   , "INFO"
											   , JOptionPane.INFORMATION_MESSAGE);
				return;//탈출
			}
			else if("".equals(jpf_pw.getText())) {
				JOptionPane.showMessageDialog(this, "비밀번호를 확인하세요."
											   , "INFO"
											   , JOptionPane.INFORMATION_MESSAGE);
				return;//탈출
			}
			else {
				String mem_id = jtf_id.getText();
				String mem_pw = jpf_pw.getText();
				BandDao bdao = new BandDao();
				nickName = bdao.procLogin(mem_id, mem_pw);
				if("아이디가 존재하지 않습니다.".equals(nickName)) {
					JOptionPane.showMessageDialog(this, "아이디를 확인하세요."
												   , "INFO"
												   , JOptionPane.INFORMATION_MESSAGE);					
				}
				else if("비밀번호가 틀립니다.".equals(nickName)) {
					JOptionPane.showMessageDialog(this, "비밀번호를 확인하세요."
												   , "WARN"
												   , JOptionPane.WARNING_MESSAGE);					
				}else {
					this.dispose();//this의 원래 화면을 닫는다.
					bc = new BandClient(nickName);
					bc.initDisplay();
					bc.init();
				}
			}
		}//end of if
		else if(obj==jbtn_join) {
			bmb = new BendMemberShip();
			bmb.setLocation(800, 250);			
		}
		
	}//end of actionPerformed
}//end of class
```

