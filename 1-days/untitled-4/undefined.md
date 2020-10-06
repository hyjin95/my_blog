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

## ChatClientVer2

### 선언부

```java
public class ChatClientVer2 extends JFrame {
	Login login = null;//인스턴스화하면 복제본이므로 생성하지 않는다.
	
	JTabbedPane jtp = new JTabbedPane();//화면분할
	WaitRoom wr = new WaitRoom(this);//클래스쪼개기, 객체주입
	MessageRoom mr = new MessageRoom(this);
	
	Socket socket = null;
	ObjectInputStream  ois = null;//듣기
	ObjectOutputStream oos = null;//말하기
	
	String nickName = null;
	///////////////////////////////end of 선언부///////////////////////////////
```

### connect\_process메서드 - 로그인성공시

```java
//접속이 성공하면 처리하는 것, 1단계 TalkClient의 init메서드와 같다.
	public void connect_process() {
		//ㅇㅇ님의 대화창
		this.setTitle(nickName+" 님의 대화창입니다.");
		//oos, ois 생성
		try {
			//소켓 인스턴스화
			socket = new Socket("192.168.0.187",4885);//소켓2	
			oos = new ObjectOutputStream(socket.getOutputStream());//소켓2 말하기구현
			ois = new ObjectInputStream(socket.getInputStream());//소켓2 듣기 구현
			
			//방 정보 초기화
			Room room = new Room();
			room.setTitle("스마트 웹모바일 엔지니어 과정 69기");//단위테스트용
			//room.current = 10;
			room.setCurrent(10);//권장사항 - 동시접속자를 처리하기 위함
			//room.state = "대기";
			room.setState("대기");
			oos.writeObject(Protocol.WAIT+Protocol.seperator+nickName
										 +Protocol.seperator+room.getState());//대기알림
			//ChatClientThreadVer2 기동 = thread기동
			ChatClientThreadVer2 cct2 = new ChatClientThreadVer2(this);
			cct2.start();
		} catch (Exception e) {
			System.out.println(e.getMessage());
		}
	}
	///////////////////////////end of connect_process//////////////////////////////
```

## WaitRoom

### 선언부

```java
/*
 * 왜 이벤트 처리시에는 인터페이스를 구현하는 것으로 설계를 했을까?
 * 버튼은 동일하지만 그 기능은 각각의 디바이스(장치)에 따라 다르다.
 * 장치마다 다르게 동작하도록 조작할 수도 있어야 할 것이다.
 * 상속을 하는 경우, 필요없는 메서드까지도 오버라이딩을 강요당할 수 있다.
 * 인터페이스의 경우에는 상속관계가 아니므로 강제조건없이 필요한 메서드만 오버라이딩할 수 있다.
 * 비교적 자유롭게 구현이 가능하고, 특정 클래스에 종속적이지 않게된다.
 * 재사용성, 이식성을 높이려면 인터페이스를 사용한다.
 * 결합도, 의존성이 낮아지므로 개발 기간을 단축할 수도 있고, 테스트 시에도 더 빠르게 해결할 수 있다.
 * 클래스 설계 시에 많이 활용되고 개발 4년차 이상 되었을때 설계를 담당한다.-업무를 알아야함
 * 게임엔진 설계나 프레임워크 개발할때에도 추상클래스나 인터페이스 중심의 설계/개발을 권장한다.
 */
public class WaitRoom extends JPanel implements ActionListener{
	
	ChatClientVer2 ccv2 = null;

	//닉네임 입력받아서 담기
	String nickName = null;
	String roomTitle = null;
	int currentNum = 1;
	Room room = null;
```

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
				}break;///////////////////////end of 방생성///////////////////////////////
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

