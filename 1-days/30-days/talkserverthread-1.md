# TalkServer

### 선언부

```java
package net.tomato_step1;

import java.net.ServerSocket;
import java.net.Socket;
import java.util.List;
import java.util.Vector;

import javax.swing.JFrame;
import javax.swing.JScrollPane;
import javax.swing.JTextArea;

public class TalkServer extends JFrame implements Runnable {
	JTextArea   jta_log = new JTextArea(10,30);//log출력용 TextArea와 세트 Scroll
	JScrollPane jsp_log = new JScrollPane(jta_log, JScrollPane.VERTICAL_SCROLLBAR_AS_NEEDED
												 , JScrollPane.HORIZONTAL_SCROLLBAR_AS_NEEDED);
	//사용자가 접속할 수 있도록 서버를 먼저 기동시킨다.
	ServerSocket server = null;//서버소켓 선언, 생성은 run메서드에서 한다. 소켓1
	//접속한 클라이언트 소켓정보를 담을 소켓을 선언한다.
	Socket client = null;//소켓3
	//List는 정해지지 않았으므로 인터페이스이다.
	List<TalkServerThread> globalList = null;//멀티스레드를 관리하는 TalkServerThread에 client에게서 들은 것을 넘긴다.
```

```java
	@Override
	public void run() {//서버소켓과 클라이언트 소켓을 연결한다.
		globalList = new Vector<>();
		boolean isStop = false;
		try {
			//서버소켓 객체 생성하기
			server = new ServerSocket(4885);//소켓1
			System.out.println("Server Ready ....\n");
			while(!isStop) {
				//서버에 접속해온 클라이언트 소켓에 대한 정보 받아내기
				//36번에서 대기상태에 빠지므로 
				//run메서드를 먼저 호출할 경우 화면을 볼 기회가 아예 없을 수 있다.
				client = server.accept();//소켓3
				jta_log.append("접속해온 사람의 정보 : "+client);
				System.out.println("접속해온 사람의 정보 : "+client);				
				System.out.println("접속해온 사람의 정보 : "+client.getInetAddress());			
				TalkServerThread tst = new TalkServerThread(this);
				tst.start();
			}//end of while
		} catch (Exception e) {
			e.printStackTrace();
		}//end of try		
	}///////////////////////////////end of run()////////////////////////
```

```java
	public static void main(String args[]) {
		TalkServer ts = new TalkServer();
		//인터페이스는 인스턴스화가 불가능하므로 반드시 생성자를 통해 넘겨야 한다.
		Thread th = new Thread(ts);
		ts.initDisplay();//run메서드 이전에 호출되어야 한다.
		th.start();//run메서드 호출 
	}/////////////////////////////////////end of main///////////////////////////////
}
```

