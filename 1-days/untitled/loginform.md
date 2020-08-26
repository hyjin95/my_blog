---
description: '이미지 넣기, 로그인, 회원가입, 인터넷 시간 받아오기'
---

# LoginForm - 카카오톡, 시계

## LoginForm.java

### import

```java
package design.book;
import java.awt.Font;
import java.awt.Graphics;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

import javax.swing.ImageIcon;
import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JLabel;
import javax.swing.JOptionPane;
import javax.swing.JPanel;
import javax.swing.JPasswordField;
import javax.swing.JTextField;
```

### 선언부 + 내 안의 클래스

```java
public class LoginForm extends JFrame implements ActionListener {
	//선언부
	String imgPath = "C:\\workspace_java\\dev_java\\src\\images\\";
	ImageIcon iicon_m = new ImageIcon(imgPath+"mains.png");
	
	JLabel jlb_id = new JLabel("아이디");
	JLabel jlb_pw = new JLabel("패스워드");
	
	JTextField jtf_id = new JTextField("test");
	JPasswordField jpf_pw = new JPasswordField("123");
	
	JPanel jp_login = new JPanel();	
	
	JButton jbtn_login = new JButton(new ImageIcon(imgPath+"login.png"));
	JButton jbtn_new = new JButton(new ImageIcon(imgPath+"new.png"));
	
	Font f = new Font("나눔고딕",Font.BOLD,17);
	
	BookManager bookMgr = null;
	LoginSub ls = null;	
	
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
```

### 화면구현부

```java
	//화면처리부
	public void initDisplay() {//setBound로 id창+tf, pw창+tf2개
		
		jbtn_login.addActionListener(this);
		jbtn_new.addActionListener(this);
		
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
		
		jbtn_new.setBounds(175, 295, 120, 40);
		jbtn_new.setFont(f);
		this.add(jbtn_new);
		
		
		this.setTitle("도서관리 시스템");
		this.setSize(350, 600);
		this.setLocation(800, 250);//띄워질 좌표 설정
		this.setVisible(true);		
	}
```

### 메인메서드

```java
	//메인메서드
	public static void main(String[] args) {
		LoginForm lf = new LoginForm();
		lf.initDisplay();
	}
```

### Override

```java
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
		else if(obj==jbtn_new) {
			ls = new LoginSub();			
		}		
	}//end of actionPerformed
}//end of class
```

## BookManager - 추가한 부분

```java
package design.book;

import timeserver.TimeClient;

//public class BookManager extends JFrame implements ActionListener{
public class BookManager extends JFrame {
	JLabel jlb_time = new JLabel("현재시간", JLabel.CENTER);
	
	public void initDisplay() {//화면그리기

		TimeClient tc = new TimeClient(jlb_time);
		tc.start();
		this.add("South", jlb_time);		
	}
```

* 시간을 표시하기 위해 추가된 부분
* import : 3번
* 선언부 : 7번
* 화면 : 11-13번

