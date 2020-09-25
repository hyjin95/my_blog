# BandServerThread

역할 : BandServer에서 Thread를 부여받은 n개의 Socket을 관리해주는 클래스  
          멀티스레드 관리 클래스

## BandServerThread.java

### 선언부

### 생성자 - 소켓동기화, 접속메세지 출력메서드

{% page-ref page="../30-days/talkserverthread.md" %}

### level 2 - run메서

```java
	public void run() {
		//System.out.println("TalkServerthread run호출성공");//단위테스트
		String msg = null;
		boolean isStop = false;
		try {
			run_start://while문을 탈출하는 label을 만든다.
			while(!isStop) {
				msg = (String)ois.readObject();
				bs.jta_log.append(msg+"\n");
				StringTokenizer st = null;
				int protocol = 0;
				if(msg!=null) {//메세지가 있다면
					st = new StringTokenizer(msg,"#");
					protocol = Integer.parseInt(st.nextToken());					
				}//end of if
				switch(protocol) {
				//default://case 없이 단위테스트하기
					//System.out.println("protocol"+protocol);
				case 200:{
					String nickName = st.nextToken();
					String message = st.nextToken();
					broadCasting(200+"#"+nickName+"#"+message);
				}break;
				case 500:{
					String nickName = st.nextToken();
					bs.globalList.remove(this);//나가기 버튼을 눌러서 여기에 들어온 스레드(this)를 삭제
					broadCasting(500+"#"+nickName);//vector에서 나간사람을 빼고 진행해야한다.
				}break run_start;//run_start라는 라벨을 적음으로써 this인 socket은 라벨을 통해 while문을 종료시킨다.
				}
			}////end of while			
		} catch (Exception e) {
			System.out.println("Exception -> "+e.toString());
		}
	}/////////////////////////////////////end of run////////////////////////////////////////////

}

```

