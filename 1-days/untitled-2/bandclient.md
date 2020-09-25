# BandClient

역할 : 접속자에게 대화창을 제공해주고, 입력값을 socket에 담아주는 클래스

## BandClient.java

```java
package net.tomato_step2;

import java.awt.BorderLayout;
import java.awt.Color;
import java.awt.Font;
import java.awt.GridLayout;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.io.ObjectInputStream;
import java.io.ObjectOutputStream;
import java.net.Socket;

import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JLabel;
import javax.swing.JOptionPane;
import javax.swing.JPanel;
import javax.swing.JScrollPane;
import javax.swing.JTable;
import javax.swing.JTextArea;
import javax.swing.JTextField;
import javax.swing.table.DefaultTableModel;

public class BandClient extends JFrame implements ActionListener{
	Socket socket = null;//소켓2
	ObjectInputStream  ois = null;//듣기
	ObjectOutputStream oos = null;//말하기
	
	JTextArea   jta_display = new JTextArea();
	JScrollPane jsp_display = new JScrollPane(jta_display, JScrollPane.VERTICAL_SCROLLBAR_AS_NEEDED
													   , JScrollPane.HORIZONTAL_SCROLLBAR_AS_NEEDED);
	
	JPanel     jp_first       = new JPanel();
	JPanel     jp_first_south = new JPanel();
	JTextField jtf_msg 	  = new JTextField(16);
	JButton    jbtn_send  	  = new JButton("전송");
	
	JPanel     jp_second       = new JPanel();
	JPanel     jp_second_south = new JPanel();
	String     cols[]          = {"대화명"};
	String     data[][]        = new String [0][1];
	DefaultTableModel dtm      = new DefaultTableModel(data, cols);
	JTable     jtb         = new JTable(dtm);
	JScrollPane jsp_nick = new JScrollPane(jtb, JScrollPane.VERTICAL_SCROLLBAR_AS_NEEDED
			   							      , JScrollPane.HORIZONTAL_SCROLLBAR_AS_NEEDED);
	JButton    jbtn_chat       = new JButton("1:1 대화");
	JButton    jbtn_change 	   = new JButton("대화명 변경");
	JButton    jbtn_emo        = new JButton("이모티콘");
	JButton    jbtn_color      = new JButton("글자색");
	JButton    jbtn_logout     = new JButton("로그아웃");
	JButton    jbtn_close      = new JButton("종료");
	
	//닉네임 입력받아서 담기
	String nickName = null;
	
	public BandClient() {}
	
	public BandClient(String nickName) {
		this.nickName = nickName;
	}
	
	public void initDisplay() {
		//사용자로부터 닉네입 입력받기		
		//nickName = JOptionPane.showInputDialog("닉네임을 입력하세요.");
		jtf_msg    .addActionListener(this);
		jbtn_send  .addActionListener(this);
	    jbtn_chat  .addActionListener(this);    
	    jbtn_change.addActionListener(this);	   
		jbtn_emo   .addActionListener(this);    
		jbtn_color   .addActionListener(this);    
		jbtn_logout   .addActionListener(this);    
		jbtn_close .addActionListener(this);
		
		jp_first_south.setLayout(new BorderLayout());
		jp_first_south.add("Center", jtf_msg);
		jp_first_south.add("East", jbtn_send);
		
		jp_first.setLayout(new BorderLayout());
		jp_first.add("Center", jsp_display);
		jp_first.add("South", jp_first_south);
		
		jp_second_south.setLayout(new GridLayout(3,2,3,3));
		jp_second_south.add(jbtn_chat);
		jp_second_south.add(jbtn_change);
		jp_second_south.add(jbtn_emo);
		jp_second_south.add(jbtn_color);
		jp_second_south.add(jbtn_logout);
		jp_second_south.add(jbtn_close);
		
		jp_second.setLayout(new BorderLayout());
		jp_second.add("Center", jsp_nick);
		jp_second.add("South", jp_second_south);
		
		jta_display.setBackground(new Color(255,217,236));
		jtf_msg.setBackground(new Color(246,246,246));
		jp_second_south.setBackground(new Color(218,217,255));
		jtb.setBackground(new Color(218,217,255));
		//jt_nick.setBackground(new Color(255,250,200));
		jbtn_chat  .setBackground(new Color(234,234,234));
		jbtn_change.setBackground(new Color(234,234,234));
		jbtn_emo   .setBackground(new Color(234,234,234));
		jbtn_color .setBackground(new Color(234,234,234));
		jbtn_logout .setBackground(new Color(234,234,234));
		jbtn_close .setBackground(new Color(234,234,234));
		jbtn_send  .setBackground(new Color(234,234,234));
		
		this.setLayout(new GridLayout(0,2,1,1));
		this.add(jp_first);
		this.add(jp_second);
		this.setTitle(nickName+" 님의 대화창입니다.");
		this.setLocation(700, 250);
		this.setSize(800, 550);
		this.setVisible(true);
	}//////////////////////////////end of initDisplay/////////////////////////////////////
	
	public void init() {
		try {
			//소켓 인스턴스화
			socket = new Socket("192.168.0.128",3007);//소켓2	
			oos = new ObjectOutputStream(socket.getOutputStream());//소켓2 말하기구현
			ois = new ObjectInputStream(socket.getInputStream());//소켓2 듣기 구현
			//서버에게 내가 입장한 사실을 알린다.말하기
			oos.writeObject(100+"#"+nickName);
			//서버의 말을 듣기 위한 준비
			BandClientThread tct = new BandClientThread(this);
			tct.start();
		} catch (Exception e) {
			System.out.println(e.getMessage());
		}		
	}//////////////////////////////////end of init////////////////////////////////////////
	
//	public static void main(String args[]) {
//		BandClient tc = new BandClient();
//		tc.initDisplay();
//		tc.init();//접속 실행		
//	}/////////////////////////////////end of main/////////////////////////////////////////

	@Override
	public void actionPerformed(ActionEvent e) {
		Object obj = e.getSource();
		String msg = jtf_msg.getText();
		if(obj==jbtn_send || obj == jtf_msg) {
			try {
				oos.writeObject(200+"#"+nickName+"#"+msg);
				jtf_msg.setText("");
			} catch (Exception e2) {
				System.out.println(e2.toString());
			}			
		}//////////////jbtn_send, jtf_msg///////////////////
		else if(obj==jbtn_close) {
			try {
				oos.writeObject(500+"#"+nickName);
				System.exit(0);
			} catch (Exception e2) {
				System.out.println(e2.toString());
			}			
		}//////////////////jbtn_close////////////////////////
		
	}/////////////////////////////end of actionPerformed//////////////////////////////////
}

```

