# 단톡방 생성, 관리 구현

## Room

{% page-ref page="../untitled-3/undefined/room.md" %}

## RoomSimulation

### main 선언부

```java
package net.tomato_step4;

import java.util.List;
import java.util.Vector;

public class RoomSimulation {

	public static void main(String[] args) {
		List<Room> roomList = new Vector<>();
		Room room = new Room();
```

### main 실행문

```java
		room.setTitle("자바 69기");
		room.setCurrent(10);
		roomList.add(room);
		List<String> nameList69 = new Vector<>();
		nameList69.add("이순신");
		nameList69.add("한석봉");
		nameList69.add("유관순");
		room.setNameList(nameList69);

		room = new Room("자바 54기",5);
		roomList.add(room);
		List<String> nameList54 = new Vector<>();
		nameList54.add("니코");
		nameList54.add("릴리아");
		nameList54.add("아리");
		room.setNameList(nameList54);
		
		room = new Room("자바 70기",13);
		roomList.add(room);		
		List<String> nameList70 = new Vector<>();
		nameList70.add("오상기");
		nameList70.add("한영진");
		nameList70.add("신민경");
		room.setNameList(nameList70);
		
		List<ChatServerThread> userList = new Vector<>();
```

### main 출력문

```java
		System.out.println("방 갯수 = "+roomList.size());//3출력
		
		for(int i=0;i<3;i++) {
			Room r = roomList.get(i);
			System.out.print(r.getCurrent()+" ");
			System.out.println(r.getTitle());
			
			for(int inwon=0;inwon<r.nameList.size();inwon++) {
				System.out.println(r.nameList.get(inwon)+" ");
			}
			System.out.println();
		}
	}
}
```

## ChatServer

### 선언부

```java
public class ChatServer extends JFrame implements Runnable, ActionListener{
    List<Room> roomList = null;
```

### run 메서드

```java
@Override
	public void run() {//서버소켓과 클라이언트 소켓을 연결한다.
		roomList = new Vector<Room>();
```

## ChatServerThread

```java
public class ChatServerThread extends Thread {
	ChatServer cs = null;
	//TalkServer에 접속해온 클라이언트 소켓 정보 담기 위해서 선언함
	Socket client = null;
	ObjectOutputStream oos = null;
	ObjectInputStream  ois = null;
	
	//현재 인원수 정보 담기, thread=사람
	int g_current = 0;	
	//현재 방 정보 담기
	String g_title = null;
	
	//접속한 접속자마다 달라지는 닉네임 선언
	String nickName = null;;
```

```java
	public ChatServerThread(ChatServer cs) {//cs룰 객체주입받는다.
		this.cs = cs;//부모의 정보를 갖게된다.
		//TalkServer에서 생성된 소켓이 필요하다.
		this.client = cs.client;//초기화 누락시:NullPointException = 소켓 동기화
		try {
			oos = new ObjectOutputStream(client.getOutputStream());//소켓3 말하기구현
			ois = new ObjectInputStream(client.getInputStream());//소켓3 듣기구현
			//사용자가 보내온 정보 읽기, 로그인 정보를 듣는다.
			String msg = (String)ois.readObject();
			cs.jta_log.append(msg+"\n");
			cs.jta_log.setCaretPosition(cs.jta_log.getDocument().getLength());
			//넘어오는정보 : 100#닉네임
			StringTokenizer st = null;
			if(msg!=null) {
				st = new StringTokenizer(msg,Protocol.seperator);
			}
			if(st.hasMoreElements()) {//boolean타입 있으면 true(진행)
				st.nextToken();//skip 100
				nickName = st.nextToken();//닉네임
				g_title = st.nextToken();//단톡방 이름
			}
			for(ChatServerThread cst : cs.globalList) {//입장하는 사람한테 모든 접속자를 알림
				String currentName = cst.nickName;
				String currentState = cst.g_title;//접속자가 입장한 방의 이름으로 초기화
				this.send(Protocol.WAIT+Protocol.seperator+currentName
									   +Protocol.seperator+currentState);//this.nickName
			}
			System.out.println("방목록 관리 : "+cs.roomList.size());//값이 제대로 담겼는지 확인
			//ROOM_LIST에 대한 처리 - 3단계 까지는 필요없던 코드
			//현재 방 정보가 없다면 필요없는 코드 이므로
			for(int i=0;i<cs.roomList.size();i++) {
				Room room = cs.roomList.get(i);//room정보 인스턴스화
				String title = room.title;
				g_title = title;
				int current =0;
				if(room.userList!=null && room.userList.size()!=0) {//접속자가 있다면
					current = room.userList.size();
				}
				g_current= current;
				this.send(Protocol.ROOM_LIST+Protocol.seperator+g_title
											+Protocol.seperator+g_current);
			}
			
			cs.globalList.add(this);//나 자신의 스레드를 add하여 서버가 나에게 간섭할 수 있다.
			//내가 입장한 후에 현재 접속자들의 접속 메세지를 처리하는 곳, 내가 처음에 입장한 경우에 실행
			this.broadCasting(msg);//내가 입력한 메세지를 보내주는 것
		} catch (Exception e) {
			System.out.println(e.toString());
		}
	}///////////////////////////////end of TalkServerThread////////////////////////////
```

## ChatClientThread

```java
public class ChatClientThreadVer2 extends Thread {

	ChatClientVer2 ccv2 = null;

	public ChatClientThreadVer2(ChatClientVer2 ccv2) {
		this.ccv2 = ccv2;
	}
```

```java
public void run() {
		boolean isStop = false;
		while(!isStop) {
			try {
				String msg = "";
				msg = (String)ccv2.ois.readObject();
				StringTokenizer st = null;//해당사항이 없을 수 있어 생성은 따로한다.
				int protocol = 0;
				if(msg!=null) {
					st = new StringTokenizer(msg,"|");
					protocol = Integer.parseInt(st.nextToken());//번호를 선언해둔 변수에 담는다.
				}
				switch(protocol) {
				case Protocol.WAIT:{
					String nickName = st.nextToken();
					String state = st.nextToken();//대기 또는 방이름이 담기는 (상태)변수
					Vector<String> v_nick = new Vector<>();
					v_nick.add(nickName);
					v_nick.add(state);
					ccv2.wr.dtm_wait.addRow(v_nick);
					//테이블 목록이 늘어날 때 자동으로 스크롤바 이동하기 구현
					ccv2.wr.jsp_wait.getVerticalScrollBar().addAdjustmentListener(new AdjustmentListener() {
						@Override
						public void adjustmentValueChanged(AdjustmentEvent e) {
							JScrollBar jsb = (JScrollBar)e.getSource();
							jsb.setValue(jsb.getMaximum());
						}						
					});
				}break;
			  }//end of switch
		  } catch (Exception e) {
				System.out.println("ChatClientThread -> "+e.toString());
				e.printStackTrace();//에러메세지의 모든 히스토리 출력 - 어디서 에러가 난 것인지 보여준다.
			}
		}//end of while		
	}////////////////////////////////////end of run//////////////////////////////////////
```

