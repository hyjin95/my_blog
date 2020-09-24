# BendServerThread



```java
package net.tomato_step2;

import java.io.ObjectInputStream;
import java.io.ObjectOutputStream;
import java.net.Socket;
import java.util.StringTokenizer;

public class BandServerThread extends Thread {
	BandServer bs = null;
	//TalkServer에 접속해온 클라이언트 소켓 정보 담기 위해서 선언함
	Socket client = null;
	ObjectOutputStream oos = null;
	ObjectInputStream  ois = null;
	
	//접속한 접속자마다 달라지는 닉네임 선언
	String nickName = null;;
	
	public BandServerThread() {}
	
	public BandServerThread(BandServer bs) {
		this.bs = bs;
		//TalkServer에서 생성된 소켓이 필요하다.
		this.client = bs.client;//초기화 누락시:NullPointException
		try {
			oos = new ObjectOutputStream(client.getOutputStream());//소켓3 말하기구현
			ois = new ObjectInputStream(client.getInputStream());//소켓3 듣기구현
			//사용자가 보내온 정보 읽기
			String msg = (String)ois.readObject();//캐스팅연산자로 타입을 맞춘다.
			//서버측 로그창에 출력해두자.-어디까지 왔고 어디서 끊겼는지 확인가능
			bs.jta_log.append(msg+"\n");
			//넘어오는정보 : 100#닉네임
			StringTokenizer st = new StringTokenizer(msg,"#");
			//첫번째는 skip하고 #다음 두번째를 닉네임으로 받아서 찍어야한다.
			st.nextToken();//스킵하고
			nickName = st.nextToken();//닉네임 담기
			//내가 입장하기 전에 입장한 사람들의 메세지를 처리하는 곳
			//=내가 가장 처음에 입장한 경우에는 아래 코드가 실행되지 않는다.
			for(BandServerThread tst : bs.globalList) {//입장해있는 사람한테 누가 입장했는지 알려주는것
				this.send(100+"#"+tst.nickName);
			}
			bs.globalList.add(this);//나 자신의 스레드를 add하는곳
			//내가 입장한 후에 입장한 사람들의 메세지를 처리하는 곳, 내가 처음에 입장한 경우에 실행
			this.broadCasting(msg);//내가 입력한 메세지를 보내주는 것
		} catch (Exception e) {
			System.out.println(e.toString());
		}
	}///////////////////////////////end of TalkServerThread///////////////////////////////////////
	
	//현재 입장해있는 접속자들 모두에게 메세지 전송하기 구현
	public void broadCasting(String msg) {
        //서버에 접속해온 접속자에 대한 정보를 입장이 일어날때마다 추가하는 코드가 필요하다.
		//어디에 어떻게? -> globalList.add(this)를 사용해야할 것이다. this=TalkServerThread
		for(BandServerThread tst : bs.globalList) {
			tst.send(msg);
			
		}
	}//////////////////////////////////end of broadCasting///////////////////////////////////////
	
	//실제로 메세지 쓰기 호출하기
	/*******************************************************************************************
	 * 파라미터로 받은메세지를 읽어서 소켓에 쓰기 처리한다.
	 * 한 사람에게만 전송할 때는 send메서드를 활용하고 여러 사람에게 전송할 때에는 broadCasting으로 처리한다.
	 * @param msg
	 *******************************************************************************************/
	public void send(String msg) {//싱글스레드용
		try {
			oos.writeObject(msg);
		} catch (Exception e) {
			e.printStackTrace();
		}
	}////////////////////////////////////end of send/////////////////////////////////////////////

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

