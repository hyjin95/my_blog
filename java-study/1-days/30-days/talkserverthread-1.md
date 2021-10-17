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
	//사용자가 접속할 수 있도록 서버를 먼저 기동
	ServerSocket server = null;
	//접속한 클라이언트 소켓정보를 담을 소켓을 선언
	Socket client = null;//소켓3
	//List는 정해지지 않았으므로 인터페이스이다.
	List<TalkServerThread> globalList = null;
```

* Server의 로그를 확인 할 수 있도록 JFrame을 상속받는다.
* run메서드 활용을 위해 Runnable인터페이스를 implements한다.
* 제일 먼저 ServerSocket생성, 선언은 run메서드에서 한다. --1번소켓
* 그 다음 클라이언트 소켓 정보를 담을 소켓을 선언한다. --3번 소켓
* Vector사용을 위해 List를 TalkServerThread타입으로 선언한다.\
  \- 멀티스레드 관리자인 tst타입으로 하는 이유는 List에 모든 접속자들을 담아야 하기 때문이다.

### Run( ) Override

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
				//대기상태 이므로 run메서드를 먼저 호출할 경우 화면을 볼 기회가 없을 수 있다.
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

* 접속 경합, 유지, 순서 처리를 위한 run메서드
* 3번 : 선언되었던 List변수를 Vector로 생성한다.
* 4번 : while문 조건에 true를 넣기위해 선언
* 5번 : try문 시
* 7번 : 서버소켓 생성 = 대문 열기 --대기상태 시작, 1번소켓 생성
* 9번 : isStop상태가 true인 동안에 -- 대기상태
* 12번 : 접속허용, 접속자의 정보를 받을 소켓 생성 및 대입 --대기상태 종료, 3번소켓 생성
* 16번 : 멀티스레드 관리 클래스가 멤버변수 list와 socket들을 사용하기 위해 인스턴스화한다.\
  \- 단, 접속자 마다 다른 thread를 부여하기위해 while문 안에 인스턴스 변수를 생성한다. 

### Main&인터페이스 인스턴스화

```java
	public static void main(String args[]) {
		TalkServer ts = new TalkServer();
		//인터페이스는 인스턴스화가 불가능하므로 반드시 생성자를 통해 넘겨야 한다.
		Thread th = new Thread(ts);
		ts.initDisplay();
		th.start();
	}/////////////////////////////////////end of main///////////////////////////////
}
```

* Runnable인터페이스로 run메서드를 활용하려면 일반 인스턴스화를 하면 안된다.
* thread의 run메서드를 구현한 자기자신의 주소번지를 Thread의 생성자 파라미터로 넘겨야한다.
* 5번 : 화면을 구현하고
* 6번 : ts에 구현된 Thread-run메서드를 실행한다.

{% content-ref url="untitled-3.md" %}
[untitled-3.md](untitled-3.md)
{% endcontent-ref %}
