# TalkServerThread

```java
package net.tomato_step1;

import java.io.ObjectInputStream;
import java.io.ObjectOutputStream;
import java.net.Socket;
import java.util.StringTokenizer;

public class TalkServerThread extends Thread {
	TalkServer ts = null;
	//TalkServer에 접속해온 클라이언트 소켓 정보 담기 위해서 선언함
	Socket client = null;
	ObjectOutputStream oos = null;
	ObjectInputStream  ois = null;
	
	//접속한 접속자마다 달라지는 닉네임 선언
	String nickName = null;;
	
	public TalkServerThread() {}
```

```java
public TalkServerThread(TalkServer ts) {
	this.ts = ts;
	//TalkServer에서 생성된 소켓이 필요하다.
	this.client = ts.client;//초기화 누락시:NullPointException
	try {
		oos = new ObjectOutputStream(client.getOutputStream());
		ois = new ObjectInputStream(client.getInputStream());
		//사용자가 보내온 정보 읽기
		String msg = (String)ois.readObject();//캐스팅연산자로 타입을 맞춘다.
		//서버측 로그창에 출력해두자.-어디까지 왔고 어디서 끊겼는지 확인가능
		ts.jta_log.append(msg+"\n");
		//넘어오는정보 : 100#닉네임
		StringTokenizer st = new StringTokenizer(msg,"#");
		//첫번째는 skip하고 #다음 두번째를 닉네임으로 받아서 찍어야한다.
		st.nextToken();//스킵하고
		nickName = st.nextToken();//닉네임 담기
		//내가 입장하기 전에 입장한 사람들의 메세지를 처리하는 곳
		//=내가 가장 처음에 입장한 경우에는 아래 코드가 실행되지 않는다.
		for(TalkServerThread tst : ts.globalList) {//입장해있는 사람한테 누가 입장했는지 알려주는것
			this.send(100+"#"+tst.nickName);
		}
		ts.globalList.add(this);//나 자신의 스레드를 add하는곳
		//내가 입장한 후에 입장한 사람들의 메세지를 처리하는 곳, 내가 처음에 입장한 경우에 실행
		this.broadCasting(msg);//내가 입력한 메세지를 보내주는 것
	} catch (Exception e) {
		System.out.println(e.toString());
	}
}/////////////////////////end of TalkServerThread/////////////////////////////
```

* 입장이 처리되는 생성자
* 22번 : 현재 접속자 Thread가 이때 Vector에 담긴다 -그래서 처음 접속하는 사람은 for문이 아닌 24번으로 메세지를 전달한다.
* 19-21번 : 두번째 접속하는 사람부터는 Vector

```java
	//현재 입장해있는 접속자들 모두에게 메세지 전송하기 구현
	public void broadCasting(String msg) {
        //서버에 접속해온 접속자에 대한 정보를 입장이 일어날때마다 추가하는 코드가 필요하다.
		//어디에 어떻게? -> globalList.add(this)를 사용해야할 것이다. this=TalkServerThread
		for(TalkServerThread tst : ts.globalList) {
			tst.send(msg);			
		}
	}//////////////////////////////////end of broadCasting///////////////////////////////////////
```

```java
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
		System.out.println("TalkServerthread run호출성공");//단위테스트
	}/////////////////////////////////////end of run////////////////////////////////////////////

}
```



