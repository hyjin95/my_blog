# TalkServerThread

### 선언부

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

* nickName은 접속자마다 그 값이 달라지지만 유지 되어야 하므로 선언은 클래스 밑에 한다.

### 생성자

```java
public TalkServerThread(TalkServer ts) {
	this.ts = ts;
	//TalkServer에서 생성된 소켓
	this.client = ts.client;//초기화 누락시:NullPointException
	try {
		oos = new ObjectOutputStream(client.getOutputStream());
		ois = new ObjectInputStream(client.getInputStream());
		//사용자가 보내온 정보 읽기
		String msg = (String)ois.readObject();
		ts.jta_log.append(msg+"\n");
		//넘어오는정보 : 100#닉네임
		StringTokenizer st = new StringTokenizer(msg,"#");
		st.nextToken();//스킵하고
		nickName = st.nextToken();//닉네임 담기
		//내가 입장하기 전에 입장한 사람들의 메세지를 처리하는 곳
		//=내가 가장 처음에 입장한 경우에는 아래 코드가 실행되지 않는다.
		for(TalkServerThread tst : ts.globalList) {
			this.send(100+"#"+tst.nickName);
		}
		ts.globalList.add(this);
		//내가 입장한 후에 입장한 사람들의 메세지를 처리하는 곳, 
		//처음에 입장한 경우에 실행
		this.broadCasting(msg);
	} catch (Exception e) {
		System.out.println(e.toString());
	}
}/////////////////////////end of TalkServerThread/////////////////////////////
```

* 입장이 처리되는 생성자
* 4-번 - TalkServer에서 생성된 소켓과 ServerSocket을 동기화
* 6-7번 - 접속자의 단말기\(Socket\)에서 말하기, 듣기 기능을 끌어온다.
* 9번  - ois로 들은 것은 Object타입이므로 캐스팅연산자를 이용해 형전환 - read되는 것은 Client가 init에서 생성한 socket에 적은,말한 Object이다.
* 10번 : ts의 로그출력화면에 메세지를 담는다. - 접속이 어디까지 이루어졌고 끊기는 곳을 확인 할 수 있다.
* 12-14번  - StringTokenizer함수를 이용해 문장 쪼개기 - 13번 : 스킵 - 14번 : 접속자를 식별하기위한 정보를 멤버변수에 담아 구분한다.
* 17-19번  - this로 접속해있는 현재 접속자에게 이전에 접속해있는 접속자들을 알려주는 구문 - 최 접속자라면 List에는 아무도 없으므로 실행되지 않는다. - n번째 접속하는 사람이 등장하면 앞선 모든 접속자들의 정보를 send함수가 나한테만\(this\) 말한다. - msg = 100\#닉네임 :  100 = 접속했다 라는 의미
* 20번  - 현재 접속자 Thread가 이때 Vector에 add된다. - 제일 처음 접속하는 사람은 23번만 들을 수 있다.
* 23번  - this로 호출한 이유 - 지금 접속자의 nickName을 받기 위함이다. - 최근 접속자의 thread에서\(this\) 멀티스레드용 전달자 broadCasting메서드로 접속한 닉네임\(this\)을를 접속해있는 n명에게 전달한다.

### broadCasting메서드

```java
public void broadCasting(String msg) { 
	for(TalkServerThread tst : ts.globalList) {
		tst.send(msg);			
	}
}////////////////////////////end of broadCasting///////////////////////////////
```

* 현재 입장해있는 모든 접속자들에게 메세지를 전송해주는 메서드이다. - 접속한 닉네임을 파라미터에 담아 현재 접속자 수만큼 말하기를 반복한다.  - for문으로 send함수 반복호출
* 첫 접속자가 등장하면 해당 접속자의 msg를 받아 한번 말한다.
* globalList는 TalkServerThread타입이다.
* globlaList는 접속이 일어날때마다 접속해온 접속자의 정보가 추가되어야 한다. - TalkServerThread생성자의 - 21번 - globalList.add\(this\) : this = TalkServerThread

### send메서드

```java
	//실제로 메세지 쓰기 호출하기
	/*****************************************************************************
	 * 파라미터로 받은메세지를 읽어서 소켓에 쓰기 처리한다.
	 * 한 사람에게 전송할 때는 send, 여러 사람에게 전송할 때에는 broadCasting으로 처리
	 * @param msg
	 *****************************************************************************/
	public void send(String msg) {//싱글스레드용
		try {
			oos.writeObject(msg);
		} catch (Exception e) {
			e.printStackTrace();
		}
	}////////////////////////////////////end of send//////////////////////////////

	public void run() {
		System.out.println("TalkServerthread run호출성공");//단위테스트
	}/////////////////////////////////////end of run/////////////////////////////

}
```

* 접속이 일어날때마다 send메서드는 두번 씩 불려진다. 
* 한번은 최근 접속자에게 현재 접속되어있는 접속자들의 모든 닉네임을 말해준다.
* 두번은 모든 현재 접속자들에게 최근 접속자의 닉네임을 말해준다.

