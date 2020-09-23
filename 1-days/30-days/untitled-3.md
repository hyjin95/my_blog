# Thread의 역할 이해하기

## 질문

#### 질문1 : run메서드 코드 구간안에는 대기, 정지상태로 빠지는 구간이 있는가?

예

#### 질문2 : 있다면, 어느 구간에서 정지상태로 빠지는가?

ServerSocket이 생성되고 나서 client가 접속해서 server.accept함수가 진행될때 까지 대기상태이다.

## 구현

### TalkServerTest - Thread가 없는 경우

```java
public class TalkServerTest extends JFrame {
public void init() {//서버소켓과 클라이언트 소켓을 연결한다.
		boolean isStop = false;
		try {
			//서버소켓 객체 생성하기
			server = new ServerSocket(4885);//소켓1
			System.out.println("Server Ready ....\n");
			while(!isStop) {
				//서버에 접속해온 클라이언트 소켓에 대한 정보 받아내기
				//35번에서 대기상태에 빠지므로 run메서드를 먼저 호출할 경우 화면을 볼 기회가 아예 없을 수 있다.
				client = server.accept();//소켓3
				jta_log.append("접속해온 사람의 정보 : "+client);
				System.out.println("접속해온 사람의 정보 : "+client);				
				System.out.println("접속해온 사람의 정보 : "+client.getInetAddress());				
			}//end of while
		} catch (Exception e) {
			e.printStackTrace();
		}//end of try		
	}
	
	public static void main(String args[]) {
		TalkServerTest tst = new TalkServerTest();
		tst.init();
		tst.initDisplay();//run메서드 이전에 호출되어야 한다.
	}
}
```

* Thread를 상속받지 않는 클래스로, Thread가 main하나 뿐이다.
* main에서 init메서드가 호출되고, init은 client접속 시까지 대기하기 때문에 화면메서드가 호출될 일이 없을 수 있다.
* client가 접속했더라도 init메서드안의 while문 때문에 머물러지는 경우에는 화면메서드를 볼 수 없게 된다.

### TalkServer - Thread가 있는 경우

```java
public class TalkServer extends JFrame implements Runnable {
@Override
	public void run() {//서버소켓과 클라이언트 소켓을 연결한다.
		boolean isStop = false;
		try {
			//서버소켓 객체 생성하기
			server = new ServerSocket(4885);//소켓1
			System.out.println("Server Ready ....\n");
			while(!isStop) {
				//서버에 접속해온 클라이언트 소켓에 대한 정보 받아내기
				//36번에서 대기상태에 빠지므로 run메서드를 먼저 호출할 경우 화면을 볼 기회가 아예 없을 수 있다.
				client = server.accept();//소켓3
				jta_log.append("접속해온 사람의 정보 : "+client);
				System.out.println("접속해온 사람의 정보 : "+client);				
				System.out.println("접속해온 사람의 정보 : "+client.getInetAddress());				
			}//end of while
		} catch (Exception e) {
			e.printStackTrace();
		}//end of try		
	}
	
	public static void main(String args[]) {
		TalkServer ts = new TalkServer();
		//인터페이스는 인스턴스화가 불가능하므로 반드시 생성자를 통해 넘겨야 한다.
		Thread th = new Thread(ts);
		ts.initDisplay();//run메서드 이전에 호출되어야 한다.
		th.start();//run메서드 호출 
	}
}
```

* JFrame을 상속받아야하므로 Thread는 Runnable인터페이스를 implements한다.
* main스레드하나. Runnable스레드 하나, 즉 스레드가 두개 생성된다.
* 스레드가 두개이므로 작업을 동시에 따로 수행이 가능하다.
* 화면메서드가 init메서드 뒤에있어도 화면을 볼 수 있다.

