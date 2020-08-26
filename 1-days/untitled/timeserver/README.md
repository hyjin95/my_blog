# TimeServer

### TimeServer

```java
package timeserver;

//TimeServer.java: 1초간격으로 현재의 시간 문자열을 접속한 클라이언트에게 전송하는 프로그램
import java.net.*;
import java.io.*;
import java.util.*;

public class TimeServer extends Thread {

   private Socket socket;//run메서드에서 사용해야하므로 멤버변수로 선언

   public TimeServer(Socket s) {//socket은 원본이여야 하므로 원본으로 초기화
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
      int port = 2001;
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

### TimeClient 

```java
package timeserver;

import java.net.*;
import java.io.*;

import javax.swing.*;

public class TimeClient extends Thread {
	private JLabel jlb_time = null;
	
   public TimeClient() {
      // TODO Auto-generated constructor stub
   }
   
   public TimeClient(JLabel jlb_time) {
	   this.jlb_time = jlb_time;
   }

   // run() 시작
   public void run() {

      Socket socket = null;
      PrintWriter out = null;
      BufferedReader in = null;
      String timeStr = null;

      try {
         socket = new Socket("192.168.0.187", 2001);
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
   // run() 종료
//   public static void main(String args[]){
//      TimeClient tc = new TimeClient();
//      tc.start();
//   }
}
```

### MovieApp - 화면

```java
package timeserver;

import java.awt.Font;

import javax.swing.JFrame;
import javax.swing.JLabel;

public class MovieApp extends JFrame{
	JLabel jlb_time = null;
	
	//타입 서버 기동시키기
	TimeServer ts = null;
	TimeClient tc = null;
	
	public MovieApp() {
		//ts = new TimeServer();
	}
	
	public void initDisplay() {
		jlb_time = new JLabel("현재시간 출력할 곳",JLabel.CENTER);
		Font f = new Font("Thoma",Font.BOLD,40);
		jlb_time.setFont(f);
		this.add("North", jlb_time);
		this.setSize(400, 300);
		this.setVisible(true);
		//주의사항 : jlb_time이 인스턴스화 되어있어야 한다. null이면 오류
		tc = new TimeClient(jlb_time);
		tc.start();
	}

	public static void main(String[] args) {
		MovieApp ma = new MovieApp();
		ma.initDisplay();
	}
}
```



