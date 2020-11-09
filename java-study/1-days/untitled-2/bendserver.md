# BandServer

역할 : Server를 열고, 접속시 Thread를 부여해주는 클래스

## BandServer.java

선언부, Run - 소켓생성, 접속 처리 메서드, main 메서드

{% page-ref page="../30-days/talkserverthread-1.md" %}

```java
package net.tomato_step2;

public class BandServer extends JFrame implements Runnable {
	JTextArea   jta_log = new JTextArea();//log출력용 TextArea와 세트 Scroll
	JScrollPane jsp_log = new JScrollPane(jta_log, JScrollPane.VERTICAL_SCROLLBAR_AS_NEEDED
												 , JScrollPane.HORIZONTAL_SCROLLBAR_AS_NEEDED);
	//사용자가 접속할 수 있도록 서버를 먼저 기동
	ServerSocket server = null;//소켓1
	//접속한 클라이언트 소켓정보를 담을 소켓을 선언
	Socket client = null;//소켓3
	
	List<BandServerThread> globalList = null;
	
	Font f = new Font("돋움체", Font.BOLD,15);
	
	public void initDisplay() {//로그 출력 화면
		jta_log.setBackground(Color.orange);
		jta_log.setFont(f);
		this.add("Center", jsp_log);
		this.setLocation(700, 250);
		this.setSize(500, 400);
		this.setVisible(true);
	}//////////////////////////end of initDisplay////////////////////////////////
	
	@Override
	public void run() {//서버소켓과 클라이언트 소켓을 연결한다.
		globalList = new Vector<>();
		boolean isStop = false;//while문에 조건이 true가 들어가야한다.
		try {
			//서버소켓 객체 생성
			server = new ServerSocket(4885);//소켓1
			while(!isStop) {
				//서버에 접속해온 클라이언트 소켓에 대한 정보 받아내기
				//대기상태
				client = server.accept();//소켓3
				jta_log.append("접속해온 사람의 정보 : "+client);
				System.out.println("접속해온 사람의 정보 : "+client);				
				System.out.println("접속해온 사람의 정보 : "+client.getInetAddress());			
				BandServerThread tst = new BandServerThread(this);//연결통로
				tst.start();//다리를 건너가게한다.
			}//end of while
		} catch (Exception e) {
			e.printStackTrace();
		}//end of try		
	}////////////////////////////end of run()/////////////////////////////
	
	public static void main(String args[]) {
		BandServer bs = new BandServer();
		//인터페이스는 인스턴스화가 불가능하므로 반드시 생성자를 통해 넘겨야 한다.
		Thread th = new Thread(bs);
		bs.initDisplay();//run메서드 이전에 호출되어야 한다.
		th.start();//run메서드 호출 
	}///////////////////////////////end of main////////////////////////////////////

}
```

* 한정된 자원 Server를 제공하는 클래스
* 서버이지만 로그출력 화면을 제공한다. - 24시간동안 일어난 모든 일에 대해 응답을 하기위한 안전장치
* JFrame : setSize, SetVisible 등 화면에 필요한 메서드를 사용할수있게 해준다.
* Runnable implements
* 27번 : 생성된 Vector는 BandServerThread에서 사용된다.            현재 메모리는 할당되었으나 List는 비어있어 v.size=0이다.
* 31번 : 생성된 ServerSocket은 대기상태이다.
* 32번 : while의 조건문에  isStop변수를 넣은것은 무한루프가 일어날 수 있는 조건true대신           변수를 선언하여 탈출장치를 마련한 것이다.
* 35번 : 접속자의 소켓정보가 만들어둔 소켓3에 담긴다.
* 39번 : 멀티스레드 관리자인 ServerThread클래스와의 연결통로 생성
* 40번 : 다리를 건너가게 한다.

