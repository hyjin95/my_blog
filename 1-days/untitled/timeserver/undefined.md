# 예제문제

## 문제

* 타임서버를 옆 친구 서버에 접속해서 출력하시오.
*  UI -&gt; JOptionPane의 InputDialog를 활용하여 ip, port번호를 받아 처리하시오.
* StringTokenzier클래스의  next\_ 생성자 중 파라미터가 두개 인것을 이용하시오.
* 입력서식 : ip\#port 
* String user = JOptionPane.showInputDialog\("'아이피주소\#포트번호'를 입력하세요."\);

## 코드

### FriendApp.java

```java
package timeserver;

import java.awt.Font;
import java.net.Socket;
import java.util.StringTokenizer;

import javax.swing.JFrame;
import javax.swing.JLabel;
import javax.swing.JOptionPane;

public class FriendApp extends JFrame{
	StringTokenizer st = null;
	JLabel jlb_time = null;
	
	//타입 서버 기동시키기
	FriendClient fc = null;
	
	public FriendApp() {		
	}//end of 생성자
	
	public void initDisplay() {
		jlb_time = new JLabel("현재시간 출력할 곳",JLabel.CENTER);
		Font f = new Font("Thoma",Font.BOLD,30);
		jlb_time.setFont(f);
		
		String ip = null;
		int port;
		String user = null;
		user = JOptionPane.showInputDialog("아이피주소#포트번호 를 입력하세요.");
		System.out.println("입력한 값 " +user);
		
		st = new StringTokenizer(user,"#");
		ip = st.nextToken();
		port = Integer.parseInt(st.nextToken("#"));
		System.out.println(ip+"/"+port);
				
		try {
			Socket socket = new Socket(ip, port);
		}catch (Exception e) {
			
		}
		//주의사항 : jlb_time이 인스턴스화 되어있어야 한다. null이면 오류
		fc = new FriendClient(jlb_time, ip, port);
		fc.start();
		
		this.add("North", jlb_time);
		this.setSize(400, 300);
		this.setVisible(true);
	}

	public static void main(String[] args) {
		FriendApp fa = new FriendApp();
		fa.initDisplay();
	}
}
```

### FriendClient.java

```java
package timeserver;

import java.net.*;
import java.io.*;

import javax.swing.*;

public class FriendClient extends Thread {
	
	private JLabel jlb_time = null;
	String ip;
	int port;	

   public FriendClient(JLabel jlb_time, String ip, int port) {
	   this.jlb_time = jlb_time;
	   this.ip = ip;
	   this.port = port;
   }

   // run() 시작
   public void run() {

	    Socket socket = null;
	    PrintWriter out = null;
	    BufferedReader in = null;
	    String timeStr = null;

	    try {
	       socket = new Socket(ip, port);
	       out = new PrintWriter(socket.getOutputStream(), true);
	       in = new BufferedReader(new InputStreamReader(socket.getInputStream()));

         while(true) {
            while((timeStr = in.readLine()) != null)//client의 듣기
               //System.out.println(timeStr);
            	jlb_time.setText(timeStr);
               //label.setText(timeStr);
            try {
               sleep(1000);
            } catch(InterruptedException i) {
               System.out.println("InterruptedException "+i.toString());
            }
         }

      } catch(IOException i) {
//         label.setText("타임서버에 접속할 수 없습니다.");
         System.out.println("타임서버에 접속할 수 없습니다.");
      } finally {
         try {
            in.close();
            out.close();
            socket.close();
         } catch (IOException e) {}
      }
   }
}//end of class
```

### FriendServer.java

```java
package timeserver;

//TimeServer.java: 1초간격으로 현재의 시간 문자열을 접속한 클라이언트에게 전송하는 프로그램
import java.net.*;
import java.io.*;
import java.util.*;

public class FriendServer extends Thread {

   private Socket socket;//run메서드에서 사용해야하므로 멤버변수로 선언

   public FriendServer(Socket s) {//socket은 원본이여야 하므로 원본으로 초기화
      this.socket = s;
   }

 // run() 시작
   public void run() {//socket사용 
      try {//원본 socket에 out과 in 만들기
         PrintWriter out = new PrintWriter(socket.getOutputStream(), true);
         BufferedReader in = new BufferedReader(new InputStreamReader(
                        socket.getInputStream()));
         while(true) {
            out.println(getTimeStr());//ln이 붙는다. 이 out은 system이 아니라 socket의 out이다. ln은 말하기가 끝났음을 표현한다.
            try {
               sleep(1000);//print하고 1초동안 sleep, 다시 print,... 반복하므로 while문, thread가 조작한다. true(상수)=반복문 탈출 불가. -> true자리에 상수아닌 변수를 써서 제어하자!!
            } catch(InterruptedException i) {}
         }

      } catch (IOException e) {
         e.printStackTrace();
      } finally {
         System.out.println("\nClient disconnected...\n");
         try {
            socket.close();
         } catch(IOException e) {}
      }
   }
 // run() 종료

 // getTimeStr() 시작
   private String getTimeStr() {
      Calendar cal = Calendar.getInstance();
      int hour = cal.get(Calendar.HOUR_OF_DAY);
      int min = cal.get(Calendar.MINUTE);
      int sec = cal.get(Calendar.SECOND);

      return (hour < 10 ? "0" + hour : "" + hour) + ":" +
            (min < 10 ? "0" + min : "" + min)  +   ":" +
            (sec < 10 ? "0" + sec : "" + sec) ;
   }
   // getTimeStr() 종료

 // main() 시작
   public static void main(String args[]) {
      int port = 20000;
      ServerSocket server = null;
      Socket client = null;
      try {
         server = new ServerSocket(port);
      } catch(IOException e) {
         System.out.println("Can't bind port: " + port);
         e.printStackTrace();
         try {
            server.close();
         } catch(IOException i) {}
         System.exit(1);
      }
      System.out.println("Time Server started successfully...");
      while(true) {//List Thread가 아니므로 인터넷이 끊기면 안내가 끊겨버린다.
         try {
            client = server.accept();//서버에 접속한 client들의 소켓들을 갖는 순간
            System.out.println("New Client connected...");
            TimeServer tServer = new TimeServer(client);
            tServer.start(); //tServer는 하나다. 한 서버가 여러 스레드를 담당한다.
            System.out.println("New Time Server Thread started...");
         } catch(IOException e) {
            System.out.println("Can't start server thread!!");
            e.printStackTrace();
            try {
               client.close();
            } catch(IOException i) {}
         }
      }
   }
 // main() 종료
}
```

