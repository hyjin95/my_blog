# LoginForm.java - 메인 화면

```java
package design.book;
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

public class LoginForm extends JFrame implements ActionListener {
	//선언부
	String imgPath = "C:\\workspace_java\\dev_java\\src\\images\\";
	ImageIcon iicon_m = new ImageIcon(imgPath+"mains.png");
	
	JLabel jlb_id = new JLabel("아이디");//"아이디"가 변경될 일이 없으므로 멤버변수로 선언,생성한다.
	JLabel jlb_pw = new JLabel("패스워드");
	
	JTextField jtf_id = new JTextField("test");
	JPasswordField jpf_pw = new JPasswordField("123");
	
	JPanel jp_login = new JPanel();	
	
	JButton jbtn_login = new JButton(new ImageIcon(imgPath+"login.png"));
	JButton jbtn_join = new JButton(new ImageIcon(imgPath+"new.png"));
	
	Font f = new Font("나눔고딕",Font.BOLD,17);
	
	BookManager bookMgr = null;
	MemberShip mb = null;	
	
	class MyPicture extends JPanel {
		public void paintComponent(Graphics g) {//paintComponent그리기 클래스 파라미터는 반드시 Graphics g
			g.drawImage(iicon_m.getImage(), 0, 0, null);
			setOpaque(false);//옵션값 죽이기
			super.paintComponent(g);//super=부모
		}
	}
	
	//생성자
	public LoginForm() {
		
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
		LoginForm lf = new LoginForm();
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
			else {
				this.dispose();//this의 원래 화면을 닫는다.
				bookMgr = new BookManager();
				bookMgr.initDisplay();
			}
		}//end of if
		else if(obj==jbtn_join) {
			mb = new MemberShip();
			mb.setLocation(800, 250);			
		}
		
	}//end of actionPerformed
}//end of class
```

