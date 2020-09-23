# TalkClient

### 선언부

```java
package net.tomato_step1;

import java.awt.BorderLayout;
import java.awt.Color;
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

public class TalkClient extends JFrame implements ActionListener{
	Socket socket = null;//소켓2
	ObjectInputStream  ois = null;//듣기
	ObjectOutputStream oos = null;//말하기
	
	//닉네임 입력받아서 담기
	String nickName = null;
```

* 23번 : Socket 선언 --2번 소켓
* 28번 : nickName은 접속자마다 달라지되 접속중에는 유지되어야 하므로 멤버변수로 선언한다.

### init 메서드 - 접속구현

```java
public void init() {
		try {
			//소켓 인스턴스화
			socket = new Socket("192.168.0.187",4885);//소켓2	
			oos = new ObjectOutputStream(socket.getOutputStream());
			ois = new ObjectInputStream(socket.getInputStream());
			//서버에게 내가 입장한 사실을 알린다.
			oos.writeObject(100+"#"+nickName);
		} catch (Exception e) {
			System.out.println(e.getMessage());
		}		
	}/////////////////////////////end of init/////////////////////////////////
```

* Client에서는 한명만 관리되므로 경합이 일어나지 않아 run메서드가 아닌 일반 메서드로 구현
* 접속할 socket에 서버의 ip와 port번호를 담아 생성한다. --소켓 2 생성
* 5-6번 : 말하기, 듣기 기능을 생성한다.
* 8번 : 서버에게 접속을 알리는 말하기

### main메서드

```java
public static void main(String args[]) {
		TalkClient tc = new TalkClient();
		tc.initDisplay();
		tc.init();
	}/////////////////////////////////end of main//////////////////////////////
```

* 3번 : 화면을 구현하고
* 4번 : init메서드\(접속\)를 실행한다. 

