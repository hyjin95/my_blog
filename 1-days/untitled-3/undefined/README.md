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

### 로그인 성공시 처리 메서드

```java
	//접속이 성공하면 처리하는 것
	public void connect_process() {
		//ㅇㅇ님의 대화창
		this.setTitle(nickName+" 님의 대화창입니다.");
		try {
			//소켓 인스턴스화, oos, ois 생성
			socket = new Socket("192.168.0.187",4885);
			oos = new ObjectOutputStream(socket.getOutputStream());
			ois = new ObjectInputStream(socket.getInputStream());
			//서버에게 내가 입장한 사실을 알린다.말하기
			oos.writeObject(Protocol.WAIT+"#"+nickName);
			ChatClientThread tct = new ChatClientThread(this);
			tct.start();
```

* 4번 : this\(JFrame\)의 title설정
* 7-9번 : 접속자의 소켓으로 말하기, 듣기 구현
* 11번 : 로그인 성공 알림 말하기, 로그인 후 일단 대기 상태이므로 Protocol.WAIT변수를 사용
* 13,14번 : 서버의 말하기를 듣는 ClientThread클래스가 말해주는 것을 듣기위한 준비 - \(this\)로 '나'와 동기화, 연동한다.

###  방 초기화

```java
		//방 정보 초기화
		Room room = new Room();
		room.setTitle("스마트 웹모바일 엔지니어 과정 69기");//단위테스트용
		//room.current = 10;
		room.setCurrent(10);//권장사항 - 동시접속자를 처리하기 위함
		//room.state = "대기";
		room.setState("대기");
		
		//ChatClientThreadVer2 기동 = thread기동
		ChatClientThreadVer2 cct2 = new ChatClientThreadVer2(this);
		
		} catch (Exception e) {
			System.out.println(e.getMessage());
		}
	}
	////////////////////////////end of connect_process///////////////////////
```

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

## WaitRoom

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
	Login login = null;//인스턴스화하면 복제본이므로 생성하지 않는다.
	
	JTabbedPane jtp = new JTabbedPane();//화면분할
	WaitRoom wr = new WaitRoom(this);//클래스쪼개기, 객체주입
	MessageRoom mr = new MessageRoom(this);
	
	Socket socket = null;
	ObjectInputStream  ois = null;//듣기
	ObjectOutputStream oos = null;//말하기
	
	String nickName = null;
	/////////////////////////////////////end of 선언부////////////////////////////////////////
	
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
	}///////////////////////////////////end of 생성자//////////////////////////////////////
	
	//접속이 성공하면 처리하는 것
	public void connect_process() {
		//ㅇㅇ님의 대화창
		this.setTitle(nickName+" 님의 대화창입니다.");
		//oos, ois 생성
		try {
			//소켓 인스턴스화
			socket = new Socket("192.168.0.187",4885);//소켓2	
			oos = new ObjectOutputStream(socket.getOutputStream());//소켓2 말하기구현
			ois = new ObjectInputStream(socket.getInputStream());//소켓2 듣기 구현
			//서버에게 내가 입장한 사실을 알린다.말하기
			oos.writeObject(Protocol.WAIT+"#"+nickName);
			//서버의 말을 듣기 위한 준비
			ChatClientThread tct = new ChatClientThread(this);
			tct.start();
			
		//방 정보 초기화
		Room room = new Room();
		room.setTitle("스마트 웹모바일 엔지니어 과정 69기");//단위테스트용
		//room.current = 10;
		room.setCurrent(10);//권장사항 - 동시접속자를 처리하기 위함
		//room.state = "대기";
		room.setState("대기");
		oos.writeObject(Protocol.WAIT+Protocol.seperator+nickName+Protocol.seperator+room.getState());//대기알림
		//ChatClientThreadVer2 기동 = thread기동
		ChatClientThreadVer2 cct2 = new ChatClientThreadVer2(this);
		cct2.start();
		} catch (Exception e) {
			System.out.println(e.getMessage());
		}
	}
	/////////////////////////////end of connect_process//////////////////////////////////
	//화면처리
	public void initDisplay() {
		//JFrame의 디폴트 레이아웃 BorderLayout 뭉개기
		Container con = this.getContentPane();
		this.getContentPane().setLayout(null);
		this.getContentPane().setBackground(new Color(203,200,255));
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

