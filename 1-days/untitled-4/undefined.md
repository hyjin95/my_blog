# 대기실&톡방 생성,관리 구현

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

* 9번 : Room의 getter, setter를 받아오기 위해 Room타입의 Vector를 생성한다.
* 10번 : Room 클래스 인스턴스화 선언, 생성

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

* 테스트를 위해 세가지 경우를 만들어 보았다.
* 1,2번 : 대기실 화면에 표시할  정보는 방이름\(Title\)과 현재정원\(current\)이다.
* 3번 : 방이 생성되면 여러 방을 관리하는 List에 add한다.
* 4번 : String타입을 갖는 List타입의 Vector를 생성해 입장하는 접속자들의 이름을 담는다.
* 5번 : room클래스의 이름을관리하는 List인 nameList에 위 Vector를 set한다.
* 26번 : 접속자들의 정보 스레드를 관리할수 있는 ServerThread타입의 Vector를 생성한다.

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

* 1번 : roomList는 그룹방이 생성될때마다 추가 되는 Vector이므로 size를 출력해보면 만들어진 그룹방의 갯수를 알 수 있다.
* 3번 : 모든 방의 정보를 출력하기위해 roomList.size값 만큼 반복한다.
* 4번 : Room 타입 r변수에 roomList에서 인덱스값 방의 정보를 꺼내 담는다.
* 5,6번 : 위에서 정보가 담긴 Room타입 r변수에서 current\(현재인원\)과 방이름을 꺼내 출력한다.
* 8번 : 방마다 그룹방안의 접속자 이름을 모두 출력하는 for문 - Room타입 r에 담긴 nameList의 size만큼 반복해야 모든 접속자가 표시될 수 있다.
* 9번 : Room클래스를 이용해 nameList에서 인덱스 값 방의 정보를 출력한다.

## ChatClientVer2

* 변동사항 없음

{% page-ref page="../untitled-3/undefined/" %}

## WaitRoom

### 선언부

```java
public class WaitRoom extends JPanel implements ActionListener{
	
	ChatClientVer2 ccv2 = null;

	//닉네임 입력받아서 담기
	String nickName = null;
	String roomTitle = null;
	int currentNum = 1;
	Room room = null;
```

* 이벤트를 처리하기위해 ActionListner를 impelements한다. - 인터페이스를 사용해 필요한 메서드만 구현해 사용한다.
* 대기실 화면에 필요한 정보 : 구분하기위한 닉네임, 방이름, 현재정원
* 위의 정보를 사용하기위해 그룹방관리 클래스이 dfjaskd\dls ddpddfkjdsklajfkld

  속을 하는 경우, 필요없는 메서드까지도 오버라이딩을 강요당할 수 있다.

  * 인터페이스의 경우

### actionPerformed - 톡방생성버튼 이벤트

```java
@Override
	public void actionPerformed(ActionEvent ae) {
		Object obj = ae.getSource();
		if(obj==jbtn_create) {
			roomTitle = JOptionPane.showInputDialog("단톡방 이름을 정하세요.");			
			if(roomTitle!=null) {
				try {
					ccv2.oos.writeObject(Protocol.ROOM_CREATAE+Protocol.seperator+roomTitle
															  +Protocol.seperator+0);
				} catch (Exception e) {
					e.printStackTrace();
				}
			}/////////end of if 단톡방 이름이 null이 아니라면/////////////////
		}///////////////////jbtn_create 단톡방 생성 버튼 클릭시//////////////////
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

### 멤버변수 선언부

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

### 생성자

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

### roomCasting메서드 - 해당톡방에만 알림전송

```java
//해당 톡방에 있는 사람들에게만 전송
	public void roomCasting(String msg, String roomTitle) {
		for(int i=0;i<cs.roomList.size();i++) {//전체 방 목록을 가져와서 비교해야한다.
			Room room = cs.roomList.get(i);
			//값이 확실한 파라미터를 앞에 써서 비교해야 nullPointer가 떨어지지 않고, 웹에서 멤버변수는 private로 선언하므로 get을 사용한다.
			if(roomTitle.equals(room.getTitle())) {//방 목록중에 이름이 파라미터와 같니?
				for(int j=0;j<room.userList.size();j++) {//해당 톡방에 접속한 사람(=thread)
					ChatServerThread cst = room.userList.get(j);
					try {
						cst.send(msg);
					} catch (Exception e) {
						e.printStackTrace();//예외메세지를 관리하는 stack영역에 메세지들을 라인번호와 함께 모두 출력해준다.
					}//end of try 
				}//end of for
			}//end of if
		}//end of for
	}///////////////////////////end of roomCasting/////////////////////////////////////
```

### run메서드 - case 방 생성

```java
	public void run() {//ChatServer에서 thread가 배분된 다음 호출된다.
		//System.out.println("TalkServerthread run호출성공");//단위테스트
		String msg = null;
		boolean isStop = false;
		try {
			run_start://while문을 탈출하는 label을 만든다.
			while(!isStop) {
				msg = (String)ois.readObject();//듣기 시작
				cs.jta_log.append(msg+"\n");
				cs.jta_log.setCaretPosition(cs.jta_log.getDocument().getLength());
				StringTokenizer st = null;//선언과 생성을 왜 분리할까?
				int protocol = 0;
				if(msg!=null) {//메세지가 있다면
					st = new StringTokenizer(msg,"|");//메세지가 없는경우에는 실행할 수 없다.
					protocol = Integer.parseInt(st.nextToken());					
				}//end of if
				switch(protocol) {//protocol이 먼저 필요하므로 if문에서 token으로 꺼내야한다.
				//default://case 없이 단위테스트하기
					//System.out.println("protocol"+protocol);
				case Protocol.ROOM_CREATAE:{//방 생성알림
					String roomTitle = st.nextToken();//방제목
					String currentNum = st.nextToken();//현재 인원수
					//Room클래스에 반영
					Room room = new Room(roomTitle, Integer.parseInt(currentNum));
					//RoomList에 추가하기
					cs.roomList.add(room);
					//대기실의 모든 접속자에게 방 생성 알림 메세지 전송
					this.broadCasting(Protocol.ROOM_CREATAE+Protocol.seperator+roomTitle
														   +Protocol.seperator+currentNum);
				}break;
				////////////////////////////end of 방생성//////////////////////////////
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
				case Protocol.ROOM_CREATAE:{
					JOptionPane.showMessageDialog(ccv2, "Client ROOM_CREATE");
					String roomTitle = st.nextToken();
					String currentNum = st.nextToken();//대기 또는 방이름
					Vector<String> v_room = new Vector<>();
					v_room.add(roomTitle);
					v_room.add(currentNum);
					ccv2.wr.dtm_room.addRow(v_room);
					ccv2.wr.jsp_room.getVerticalScrollBar().addAdjustmentListener(new AdjustmentListener() {
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

