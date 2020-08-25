# TimeServer2.jav - Socket

```java
package book.ch8;

import java.net.ServerSocket;
import java.net.Socket;
import java.util.ArrayList;
import java.util.Calendar;
import java.util.List;

import javax.swing.JFrame;
import javax.swing.JScrollPane;
import javax.swing.JTextArea;

public class TimeServer2 extends JFrame implements Runnable {
	ServerSocket server = null;
	Socket socket = null; //thread에서 사용할 수 있게 전변선언
	//Thread = 안내하는 역할, 자리를 안내하고, 메뉴도 안내하고, 하는 직원 그림 
	//내가 갔다오는것이 아니라 Thread가 갔다온다.
	List<TimeServerThread> globalList = null;
	//List타입의 golbalList 배열의 방안에는 thread가 들어있다.
	//0번째 - new TimeServerThread(); 1번째 - new TimeServerThread(); 3번째 - new TimeServerThread();
	
	JTextArea jta_log = new JTextArea();
	JScrollPane jsp_log = new JScrollPane(jta_log
											,JScrollPane.VERTICAL_SCROLLBAR_AS_NEEDED
											,JScrollPane.HORIZONTAL_SCROLLBAR_AS_NEEDED);
	
	public TimeServer2() {
		initDisplay();
	}


	@Override
	public void run() {//run메서드 안에서 서버의 socket을 열어야한다. 콜백메서드 start() -> run()
		globalList = new ArrayList<>();//내안에 있는 구현체클래스
		boolean isStop = false;
		try {
			server = new ServerSocket(3000);
			jta_log.append("Server Ready......................\n");
			while(!isStop) {
				socket= server.accept();
				jta_log.append("Client INFO "+socket.getInetAddress()+"\n");;
				
				TimeServerThread tst = new TimeServerThread(this);//스레드로 넘어가기
				tst.start();//run메서드가 thread에 탑승했다.
				//run메서드를 직접 호출 하지 않고 start하는 이유, 순서대로 대기하게 하기 위함이다. 바로나가는게 아니라 부르면 나가려고..
				//알고리즘 = 프로세스 관리 (동선짜주기, 원무과갔다가 검사실갔다가 -갔다가 오세요 같은 그림)
			}//사용자를 기다린다.
		} catch (Exception e) {
			
		}//end of try-catch		
	}//end of run
	
	//현재시간정보를 제공하는 메서드 선언-삼항연산자이용
	public String pushTime() {
		Calendar cal = Calendar.getInstance();//객체가 주입된다.
		int hour = cal.get(Calendar.HOUR_OF_DAY);
		int min  = cal.get(Calendar.MINUTE);
		int sec  = cal.get(Calendar.SECOND);
		return (hour < 10 ? "0"+hour : ""+hour)//삼항연산자
		     +":"+(min < 10 ? "0"+min : ""+min)//자릿수를 두자리로 통일한다.
		     +":"+(sec < 10 ? "0"+sec : ""+sec);
	}
	
	public void initDisplay() {
		jta_log.setEditable(false);
		this.add("Center", jsp_log);
		this.setTitle("타임서버 로그");
		this.setSize(500, 400);
		this.setVisible(true);
	}

	public static void main(String[] args) {
		TimeServer2 ts2 = new TimeServer2();
		ts2.initDisplay();
		Thread th = new Thread(ts2);
		System.out.println("현재 스레드 이름 : "+Thread.currentThread().getName());
		th.start();//run메서드가 호출된다.		
	}
}
//상속 : 나==Thread -> 나.start -> run()호출
//인터페이스 : 생성자를 이용해 따로 인스턴스화 : Thread th = new Thread(ts); -> th.start -> run()호출
```

```java
package book.ch8;

import java.io.ObjectInputStream;
import java.io.ObjectOutputStream;
import java.net.Socket;

public class TimeServerThread extends Thread {
	//TimeServer2 ts= new TimeServer2();//복제본생성, 이렇게 하지 말자.
	TimeServer2 ts = null;
	ObjectOutputStream oos = null;
	ObjectInputStream ois = null;
	Socket client = null;//Socket타입, null->기다려 넌 ts에서 정해질거야. -> 17번
	String timeStr = null;//시간
	
	public TimeServerThread(TimeServer2 ts) {//인스턴스 선언, 이 생성자가 없으면 nullpointexception일어난다.
		this.ts = ts;//전역변수로 사용하기	
		this.client = ts.socket;//중요중요!!!!!!전변 client를 ts의 socket으로 대입한다.
		timeStr = ts.pushTime();
		try {
			oos = new ObjectOutputStream(client.getOutputStream());
			ois = new ObjectInputStream(client.getInputStream());
			for(TimeServerThread tst:ts.globalList) {//중요!!!개선된 포문, 전체조회할때 사용한다. -> List에 담긴거 다 보여줘 -> 왼쪽에 나
				oos.writeObject(timeStr);//timeStr=msg
			}
			
			//현재 서버에 입장한 클라이언트 스레드 추가하기
			ts.globalList.add(this);//중요중요중요!!!
			this.broadCasting(timeStr);//중요중요중요!!!			
		} catch (Exception e) {
			System.out.println(e.toString());
		}
	}//end of 생성자
	
	//현재 입장해 있는 모든 친구들에게 모두 메세지를 전송한다.
	public void broadCasting(String Msg) {//모든 접속자에게 시간을 알려줄때까지
		for(TimeServerThread tst:ts.globalList) {
			try {
				oos.writeObject(Msg);//메세지가 간다.
			} catch (Exception e) {
				e.printStackTrace();//Stack영역에 쌓여있는 오류 메세지를 모두 출력한다.
			}
		}
	}//end of broadCasting
	
	//run메서드에는 뭘 써야하지?
	@Override
	public void run() {
		System.out.println("TimeServerThread run 호출성공");
	}
}
```

```java
package book.ch8;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.ObjectInputStream;
import java.io.ObjectOutputStream;
import java.io.PrintWriter;
import java.net.ServerSocket;
import java.net.Socket;

//클라이언트
public class TimeClient implements Runnable {
	ObjectOutputStream oos = null;
	ObjectInputStream ois = null;
	//소켓 생성
	Socket socket = null;
	
	public void init() {
		try {
			socket = new Socket("192.168.0.128"+ "", 3000);
			oos = new ObjectOutputStream(socket.getOutputStream());
			ois = new ObjectInputStream(socket.getInputStream());
			Thread th = new Thread(this);
			th.start();//run호출
		} catch (Exception e) {
			System.out.println(e.toString());
		} 
		
	}//end of init

	@Override
	public void run() {
		System.out.println("TimeClient run 호출 성공");	
		
		//서버에서 전송된 메세지 듣기부분
		try {
			String msg = (String)ois.readObject();//캐스팅연산자로 강제 형전환
			System.out.println("서버에서 읽어온 시간은 " +msg);
			
		} catch (Exception e) {
			// TODO: handle exception
		}
	}//end of run
	
	public static void main(String[] args) {
		TimeClient tc = new TimeClient();
		tc.init();//소켓 관련 정보 초기화
	}
}//end of class
```

