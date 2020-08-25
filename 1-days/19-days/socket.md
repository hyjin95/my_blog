# Socket

```java
package book.ch8;
//서버
import java.net.ServerSocket;
import java.net.Socket;
import java.util.Calendar;

public class TimeServer extends Thread {
	
	//스레드가 제공해주는 콜백메서드
	//1초에 한번씩 지속적으로 현재 시간정보를 초 단위로 제공
	@Override
	public void run() {
		System.out.println("run 호출성공");
	}
	
	//현재시간정보를 제공하는 메서드 선언
	public String pushTime() {
		Calendar cal = Calendar.getInstance();//객체가 주입된다.
		int hour = cal.get(Calendar.HOUR_OF_DAY);
		int mit  = cal.get(Calendar.MINUTE);
		int sec  = cal.get(Calendar.SECOND);
		return "13:05:25";
	}

	public static void main(String[] args) {
		int port = 8000;
		//서버는 동시에 여러사람을 관리해야한다.
		//순서를 놓치거나 어떤 사람을 배제시켜서 서비스를 제공받지 못하게 되면 안된다.
		//ServerSoket을 그래서 준다.
		//어느 한 사람도 놏피지 않고 접수를 받기 위해서 접수만 전담하는 소켓을 선언.
		ServerSocket server = null;//java.net 사용
		//클라이언트가 인스턴스화를 하면 = new Socket(192.168.0.244,8000) - ip, port
		//서버소켓에 클라이언트의 소켓의 정보를 듣게 된다. ->없을수도잇다 -> 예외처리
		Socket client = null;
		try {
			server = new ServerSocket(port);
									
		}catch(Exception e) {
			
		}
		System.out.println("Time Server started successfully.....");
		while(true) {
			try {
				client = server.accept();
				System.out.println("New Client connected...");
			}catch (Exception e) {
				
			}
		}//end of while
	}//end of main
}//end of Thread

```

```java
package book.ch8;

import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.io.PrintWriter;
import java.net.ServerSocket;
import java.net.Socket;

//클라이언트
public class TimeClient implements Runnable {

	@Override
	public void run() {
		System.out.println("run 호출 성공");	
		//소켓 생성
		Socket socket = null;
		PrintWriter out = null;//말할때
		BufferedReader in = null;//들을때
		try {
			socket = new Socket("192.168.0.128"+ "", 8000);
			out = new PrintWriter(socket.getOutputStream(), true);
			in = new BufferedReader( new InputStreamReader(socket.getInputStream()));
		}catch (Exception e) {
			
		}
	}
	
	public static void main(String[] args) {
		TimeClient tc = new TimeClient();
		Thread th = new Thread(tc);//인터페이스는 인스턴스화가 불가능하므로 반드시 생성자를 통해 넘겨야 한다.
		th.start();//콜벡메서드 호출하기
	
	}

}
```

