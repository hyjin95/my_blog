# TalkClient

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

```java
public void initDisplay() {
	//사용자로부터 닉네입 입력받기
	nickName = JOptionPane.showInputDialog("닉네임을 입력하세요.");
}
```

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

```java
public static void main(String args[]) {
		TalkClient tc = new TalkClient();
		tc.initDisplay();
		tc.init();//접속 실행		
	}/////////////////////////////////end of main//////////////////////////////
```

