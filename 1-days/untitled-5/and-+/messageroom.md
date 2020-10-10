# MessageRoom 구현

### 선언부

```java
package net.tomato_step4;

import java.awt.BorderLayout;
import java.awt.Color;
import java.awt.GridLayout;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JOptionPane;
import javax.swing.JPanel;
import javax.swing.JScrollPane;
import javax.swing.JTable;
import javax.swing.JTextArea;
import javax.swing.JTextField;
import javax.swing.table.DefaultTableModel;

public class MessageRoom extends JPanel implements ActionListener{
//public class MessageRoom extends JFrame implements ActionListener{

	ChatClientVer2 ccv2 = null;
	
	JTextArea   jta_client = new JTextArea();
	JScrollPane jsp_client = new JScrollPane(jta_client, JScrollPane.VERTICAL_SCROLLBAR_AS_NEEDED
													   , JScrollPane.HORIZONTAL_SCROLLBAR_AS_NEEDED);
	JPanel     jp_first       = new JPanel();
	JPanel     jp_first_south = new JPanel();
	JTextField jtf_client 	  = new JTextField(16);
	JButton    jbtn_send  	  = new JButton("전송");
	
	JPanel     jp_second       = new JPanel();
	JPanel     jp_second_south = new JPanel();
	String     cols[]          = {"대화명"};
	String     data[][]        = new String [0][1];
	DefaultTableModel dtm      = new DefaultTableModel(data, cols);
	JTable     jt_nick         = new JTable(dtm);
	JButton    jbtn_chat       = new JButton("1:1 대화");
	JButton    jbtn_change 	   = new JButton("대화명 변경");
	JButton    jbtn_emo        = new JButton("이모티콘");
	JButton    jbtn_close      = new JButton("종료");
```

### 생성자

```java
	public MessageRoom() {}
	public MessageRoom(ChatClientVer2 ccv2) {
		this.ccv2 = ccv2;
		initDisplay();
	}
```

### 화면구현

```java
	public void initDisplay() {
		jbtn_send  .addActionListener(this);
	    jbtn_chat  .addActionListener(this);    
	    jbtn_change.addActionListener(this);	   
		jbtn_emo   .addActionListener(this);    
		jbtn_close .addActionListener(this);
		
		jp_first_south.setLayout(new BorderLayout());
		jp_first_south.add("Center", jtf_client);
		jp_first_south.add("East", jbtn_send);
		
		jp_first.setLayout(new BorderLayout());
		jp_first.add("Center", jsp_client);
		jp_first.add("South", jp_first_south);
		
		jp_second_south.setLayout(new GridLayout(2,2,3,3));
		jp_second_south.add(jbtn_chat);
		jp_second_south.add(jbtn_change);
		jp_second_south.add(jbtn_emo);
		jp_second_south.add(jbtn_close);
		
		jp_second.setLayout(new BorderLayout());
		jp_second.add("Center", jt_nick);
		jp_second.add("South", jp_second_south);
		
		jta_client.setBackground(new Color(255,217,236));
		jtf_client.setBackground(new Color(246,246,246));
		jp_second_south.setBackground(new Color(218,217,255));
		jt_nick.setBackground(new Color(218,217,255));
		//jt_nick.setBackground(new Color(255,250,200));
		jbtn_chat  .setBackground(new Color(234,234,234));
		jbtn_change.setBackground(new Color(234,234,234));
		jbtn_emo   .setBackground(new Color(234,234,234));
		jbtn_close .setBackground(new Color(234,234,234));
		jbtn_send  .setBackground(new Color(234,234,234));
		
		this.setLayout(new GridLayout(0,2,1,1));
		this.add(jp_first);
		this.add(jp_second);
		this.setLocation(700, 250);
		this.setSize(600, 500);
		this.setVisible(true);
	}
```

### main 메서드

```java
	public static void main(String[] args) {
		MessageRoom mr = new MessageRoom();
		mr.initDisplay();
	}
```

### actionPerformed

```java
	@Override
	public void actionPerformed(ActionEvent e) {
		Object object = e.getSource();
		
	}
}
```



