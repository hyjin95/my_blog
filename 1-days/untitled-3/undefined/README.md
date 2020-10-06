# 대기실 구현\(상\)

## Login

### 선언부

```java
public class Login extends JFrame implements ActionListener {
    ChatClientVer2 ccv2 = null;
    String nickName = null;
```

### actionPerformed - 로그인성공

```java
@Override
	public void actionPerformed(ActionEvent e) {
		Object obj = e.getSource();//이벤트가 감지된 주소번지 달기
		}else {
					jtf_id.setText("");
					jpf_pw.setText("");
					this.dispose();//this의 원래 화면을 닫는다.
					JOptionPane.showMessageDialog(this, nickName + "님의 접속을 환영합니다."
													  , "INFO"
													  , JOptionPane.INFORMATION_MESSAGE);					
					ccv2 = new ChatClientVer2(this);
				}
```

* 새로 만든 ChatClientVer2클래스와 연결한다.
* 멤버변수 nickName을 사용하기위해 생성자\(this\)로 넘긴다.

## ChatClientVer2

### 선언부

```java
package net.tomato_step4;

import java.awt.Color;
import java.awt.Container;
import java.awt.Font;
import java.io.ObjectInputStream;
import java.io.ObjectOutputStream;
import java.net.Socket;

import javax.swing.JFrame;
import javax.swing.JTabbedPane;

public class ChatClientVer2 extends JFrame {
	Login login = null;//복제본을 생성하지 않는다.
	
	JTabbedPane jtp = new JTabbedPane();//화면분할
	WaitRoom wr = new WaitRoom(this);//클래스쪼개기, 객체주입
	MessageRoom mr = new MessageRoom(this);
	
	Socket socket = null;
	ObjectInputStream  ois = null;//듣기
	ObjectOutputStream oos = null;//말하기
	
	String nickName = null;
	///////////////////////////////end of 선언부/////////////////////////////////
```

* JFrame을 상속하는 패널을 붙일 메인 컨테이너 클래스
* 14번 : Login의 멤버변수 nickName의 원본을 사용하기 위해 생성자로 넘겨 생성한다.
* 16번 : 탭형식으로 화면을 구현하기위해 JTabbendPane클래스를 선언, 생성한다.
* 17,18번 : 탭에 붙일 패널을 멤버변수 nickName을 공유하기 위해 생성자\(this\)로 넘겨 선언, 생성한다.

### 생성자

```java
	public ChatClientVer2() {}
	
	public ChatClientVer2(Login login) {
		this.login = login;
		this.nickName = login.nickName;//접속자 동기화
		try {
			initDisplay();
			connect_process();
		}catch (Exception e) {
			System.out.println(e.toString());
		}
	}///////////////////////////////end of 생성자/////////////////////////////////
```

* 3,4번 : 생성자 파라미터로 Login을 받아 원본을 연결한다.
* 5번 : Login의 멤버변수 원본을 받아 접속자를 동기화한다.
* 6-11번 : 화면을 호출하고 말하기를 실행\(서버와의 연동\)이 일어나므로 예외처리는 필수

### 로그인 성공처리, 방 초기화

```java
	public void connect_process() {
		//ㅇㅇ님의 대화창
		this.setTitle(nickName+" 님의 대화창입니다.");
		//oos, ois 생성
		try {
			//소켓 인스턴스화
			socket = new Socket("192.168.0.187",4885);//소켓2	
			oos = new ObjectOutputStream(socket.getOutputStream());//소켓2 말하기구현
			ois = new ObjectInputStream(socket.getInputStream());//소켓2 듣기 구현
			
			//방 정보 초기화
			Room room = new Room();
			room.setTitle("스마트 웹모바일 엔지니어 과정 69기");//단위테스트용
			//room.current = 10;
			room.setCurrent(10);//권장사항 - 동시접속자를 처리하기 위함
			//room.state = "대기";
			room.setState("대기");
			oos.writeObject(Protocol.WAIT+Protocol.seperator+nickName
										 +Protocol.seperator+room.getState());//대기알림
			//ChatClientThreadVer2 기동 = thread기동
			ChatClientThreadVer2 cct2 = new ChatClientThreadVer2(this);
			cct2.start();
		} catch (Exception e) {
			System.out.println(e.getMessage());
		}
	}
	////////////////////////////end of connect_process/////////////////////////////
```

* 3번 : this\(JFrame\)의 title설정
* 7-9번 : 접속자의 소켓으로 말하기, 듣기 구현
* 12번 : 단톡방 정보를 저장하는 저장소클래스 Room타입의 변수를 선언, 생성한다.
* 13-17번 : room변수에 필요한 값을 저장, 초기한다.  - title\(단톡방명\), current\(현재 정원\), state\(상태\)
* 18번 : 로그인 성공 알림 말하기, 로그인 후 일단 대기 상태이므로 Protocol.WAIT변수를 사용
* 21번 : 서버의 말하기를 듣는 ClientThread클래스가 말해주는 것을 듣기위한 준비 - \(this\)로 접속자와 동기화, 연동한다.

{% page-ref page="room.md" %}

{% page-ref page="protocol.md" %}



### JFrame화면 구현&main

```java
	//화면처리
	public void initDisplay() {
		//JFrame의 디폴트 레이아웃 BorderLayout 뭉개기
		Container con = this.getContentPane();
		this.getContentPane().setLayout(null);
		this.getContentPane().setBackground(new Color(176,173,250));
		jtp.addTab("대기실", wr);
		jtp.addTab("대화방", mr);
		jtp.setFont(new Font("SansSerif",0,15));
		jtp.setBounds(5, 4, 630, 530);
		this.getContentPane().add(jtp,null);//화면 초기화용
		this.setSize(660, 585);
		this.setVisible(true);
		
	}///////////////////////////////end of initDisplay///////////////////////////////////

	public static void main(String[] args) {
		ChatClientVer2 ccv2 = new ChatClientVer2();
		ccv2.initDisplay();
	}/////////////////////////////////end of main////////////////////////////////////////
}
```

* 5번 : this\(JFrame\)의 패널을 getContentPane함수를 이용해 얻어 Layout을 뭉갠다.
* 7,8번 : JTabbedPane클래스가 제공하는 addTabl함수를 이용해 만들어둔 대기실클래스\(JPanel\) , 대화방 클래스 화면\(JPanel\)을 tab의 형식으로 JFrame에 붙인다.
* 11번 : 접속자마다 새로운 화면을 보여줘야 하므로 화면을 초기화한다. - this\(JFrame\)의 패널을 얻어 null인 Tab을 붙인다.

## WaitRoom

### 선언부

```java
package net.tomato_step4;

import java.awt.BorderLayout;
import java.awt.Color;
import java.awt.GridLayout;
import java.awt.Image;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;
import java.io.ObjectInputStream;
import java.io.ObjectOutputStream;
import java.net.Socket;

import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JLabel;
import javax.swing.JPanel;
import javax.swing.JScrollPane;
import javax.swing.JTable;
import javax.swing.JTextField;
import javax.swing.JTextPane;
import javax.swing.table.DefaultTableModel;
import javax.swing.text.DefaultStyledDocument;
import javax.swing.text.StyleContext;
import javax.swing.text.StyledDocument;

public class WaitRoom extends JPanel implements ActionListener{
//public class WaitRoom extends JFrame implements ActionListener{
	
	ChatClientVer2 ccv2 = null;
	
	JPanel     jp_first        = new JPanel();
	String     cols[]          = {"대화명", "위치"};
	String     data[][]        = new String [0][2];
	DefaultTableModel dtm_wait = new DefaultTableModel(data, cols);
	JTable      jtb_wait       = new JTable(dtm_wait);
	JScrollPane jsp_wait	   = new JScrollPane(jtb_wait, JScrollPane.VERTICAL_SCROLLBAR_AS_NEEDED
														 , JScrollPane.HORIZONTAL_SCROLLBAR_AS_NEEDED);
		
	JPanel     jp_second       = new JPanel();
	JPanel     jp_second_south = new JPanel();
	String     cols2[]         = {"대화방 이름","현재 인원"};
	String     data2[][]       = new String [0][2];
	DefaultTableModel dtm_room = new DefaultTableModel(data2, cols2);
	JTable      jtb_room       = new JTable(dtm_room);
	JScrollPane jsp_room       = new JScrollPane(jtb_room, JScrollPane.VERTICAL_SCROLLBAR_AS_NEEDED
														 , JScrollPane.HORIZONTAL_SCROLLBAR_AS_NEEDED);
	
	JButton    jbtn_chat   = new JButton("톡방생성");
	JButton    jbtn_in 	   = new JButton("입장하기");
	JButton    jbtn_out    = new JButton("나가기");
	JButton    jbtn_close  = new JButton("종료");
	
	//닉네임 입력받아서 담기
	String nickName = null;
	String roomTitle = null;
	int currentNum = 1;
	Room room = null;
```

* 단톡방 마다 다른 값을 갖는 변수를 멤버변수에 선언한다. - nickName    접속자 닉네임 - roomTitle     단톡방 이름 - currentNum 현재 정원 - room             방 정보

### 생성자&화면구현

```java
	public WaitRoom() {}
	
	//나는 생성자 속에서 다른 클래스와의 조립관계에서 파생되는 일들을 초기화하거나 호출 할 수 있다.
	public WaitRoom(ChatClientVer2 ccv2) {
		this.ccv2 = ccv2;
		//chatClientver2가 메모리에 로딩 될ㄸ#ㅐ 이 객체도 같이 활성화되어있어야 한다.
		initDisplay();
	}

	public void initDisplay() {

	    jbtn_chat  .addActionListener(this);    
	    jbtn_in    .addActionListener(this);	   
		jbtn_out   .addActionListener(this);    
		jbtn_close .addActionListener(this);
		
		jp_first.setLayout(new BorderLayout());
		jp_first.add("Center", jsp_wait);
		
		jp_second_south.setLayout(new GridLayout(2,2,3,3));
		jp_second_south.add(jbtn_chat);
		jp_second_south.add(jbtn_in);
		jp_second_south.add(jbtn_out);
		jp_second_south.add(jbtn_close);
		
		jp_second.setLayout(new BorderLayout());
		jp_second.add("Center", jsp_room);
		jp_second.add("South", jp_second_south);
		
		//jta_display.setBackground(new Color(255,217,236));
		jp_second_south.setBackground(new Color(246,246,246));
		jtb_room.setBackground(new Color(218,217,255));
		//jt_nick.setBackground(new Color(255,250,200));
		jbtn_chat  .setBackground(new Color(234,234,234));
		jbtn_in    .setBackground(new Color(234,234,234));
		jbtn_out   .setBackground(new Color(234,234,234));
		jbtn_close .setBackground(new Color(234,234,234));
		
		this.setLayout(new GridLayout(0,2,1,1));
		this.add(jp_first);
		this.add(jp_second);
		//this.setTitle(nickName+" 님의 대기실입니다.");
		this.setLocation(700, 250);
		this.setSize(600, 500);
		this.setVisible(true);
	}//////////////////////////////end of initDisplay/////////////////////////////////////

	public static void main(String[] args) {
		WaitRoom wr = new WaitRoom();
		wr.initDisplay();
	}

	@Override
	public void actionPerformed(ActionEvent e) {
		// TODO Auto-generated method stub
		
	}

}
```

## MessageRoom

```java
package net.tomato_step4;

import javax.swing.JFrame;
import javax.swing.JPanel;

public class MessageRoom extends JPanel{
//public class MessageRoom extends JFrmae{

	ChatClientVer2 ccv2 = null;

	public MessageRoom() {}
	public MessageRoom(ChatClientVer2 ccv2) {
		this.ccv2 = ccv2;
	}
	
	public void initDisplay() {
		this.setSize(600, 500);
		this.setVisible(true);
	}
	
	public static void main(String[] args) {
		MessageRoom mr = new MessageRoom();
		mr.initDisplay();
	}
}
```

* 토대만 구현, 36일차에 완성한다.

