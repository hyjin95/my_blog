# Thread의 역할 이해하기

### TalkServerTest.java

```java
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
```

