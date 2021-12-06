# 대기실&톡방 생성,관리 구현

## Room

{% content-ref url="../untitled-3/undefined/room.md" %}
[room.md](../untitled-3/undefined/room.md)
{% endcontent-ref %}

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
* 1,2번 : 대기실 화면에 표시할  정보는 방이름(Title)과 현재정원(current)이다.
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
* 5,6번 : 위에서 정보가 담긴 Room타입 r변수에서 current(현재인원)과 방이름을 꺼내 출력한다.
* 8번 : 방마다 그룹방안의 접속자 이름을 모두 출력하는 for문\
  \- Room타입 r에 담긴 nameList의 size만큼 반복해야 모든 접속자가 표시될 수 있다.
* 9번 : Room클래스를 이용해 nameList에서 인덱스 값 방의 정보를 출력한다.

## ChatClientVer2

```java
public void connect_process() {
		//ㅇㅇ님의 대화창
		this.setTitle(nickName+" 님의 대기실입니다.");
		//oos, ois 생성
		try {
			//소켓 인스턴스화
			socket = new Socket("192.168.123.7",4885);//소켓2	
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
	//////////////////////////end of connect_process/////////////////////////////
```

* 변동사항 없음

{% content-ref url="../untitled-3/undefined/" %}
[undefined](../untitled-3/undefined/)
{% endcontent-ref %}

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

* 이벤트를 처리하기위해 ActionListner를 impelements한다.\
  \- 인터페이스를 사용해 필요한 메서드만 구현해 사용한다.
* 대기실 화면에 필요한 정보 : 구분하기위한 닉네임, 방이름, 현재정원
* 위의 정보를 사용하기위해 그룹방관리 클래스인 Room클래스의 인스턴스를 선언해 놓는다.

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

* 톡방생성버튼에 대한 이벤트 구현부분
* 5번 : 버튼이 눌리면 이름을 입력할수 있는 창을 띄운다.
* 6-9번 : 이름이 입력되면 JFrame에 있는 소켓의 oos를 가져와 말하기 한다.\
  \- ChatClientVer2클래스에 구현된 oos를 사용해서 writeObject한다.\
  \- 방생성 구분번호 = Protocol.ROOM\_CREATE\
  \- 필요한 정보 : 구분번호, 방이름, 방인원\
  \- 방인원은 default가 0명이므로 0을 전송한다.

## ChatServer

### 선언부

```java
public class ChatServer extends JFrame implements Runnable, ActionListener{
    List<Room> roomList = null;
```

* 방 목록 관리를 위한 Room클래스 타입의 List를 멤버변수로 선언한다.
* ChatServer클래스에서 생성해야 관리가 가능하다 ServerThread는 스레드관리 클래스이므로

### run 메서드

```java
@Override
	public void run() {//서버소켓과 클라이언트 소켓을 연결한다.
		roomList = new Vector<Room>();
```

* run메서드 안에 방 목록 List를 Vector로 생성한다.

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

* clientThread로 넘겨야하는 정보인 현재 그룹방의 정보, 현재 인원수를 담는 변수를 멤버변수로 선언한다.

### 생성자 - 접속성공시 실행

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
```

* 기존과 동일한 부분
* msg = (Protocol.WAIT+|+nickName+|+room.getState( ) );\
  \- state의 default는 "대기"이다.

### 생성자 - 접속시 내정보 띄우기&#x20;

```java
			StringTokenizer st = null;
			if(msg!=null) {
				st = new StringTokenizer(msg,Protocol.seperator);
			}
			if(st.hasMoreElements()) {
				st.nextToken();//skip 100
				nickName = st.nextToken();//닉네임
				g_title = st.nextToken();//단톡방 이름 or "대기"
			}
			for(ChatServerThread cst : cs.globalList) {
				String currentName = cst.nickName;
				String currentState = cst.g_title;//접속자가 입장한 방의 이름으로 초기화
				this.send(Protocol.WAIT+Protocol.seperator+currentName
									   +Protocol.seperator+currentState);//this.nickName
			}
```

* 생성자는 대기실에 입장과 동시에 이루어지므로 화면에 필요한 정보를 담아서 말해야한다.\
  \- 닉네임, 현재위치, 그룹방이름, 현재 정원
* 1-4번 : StringTokenizer를 선언과 생성을 분리한다.\
  \- msg가 존재하는때에만 진행한다.
* 5번 : msg가 존재하고 StringTokenizer에 다음 값이 존재 한다면,\
  \- hasMoreElements( )함수는 boolean타입으로 남은 값이 있다면 true를 반환하여 구현문을 진행한다.
* 7,8번 : 닉네임, 그룹방이름 멤버변수를 가져온 값으로 초기화한다.\
  \- g\_title의 값은 은 "대기"아니면 현재접속자가 입장한 그룹방의 이름이다.
* 10번 : ChatServer의 globalList, 즉 현재 접속자 모두만큼 반복한다.
* 11,12번 : 접속자의 이름과 상태를 담을 변수를 선언, 생성한다. - 반복
* 13번 : 현재 멤버변수 nickName인 '나'에게 send한다. -반복\
  \- **'나'에게 모든 대기실 접속자의 닉네임과 상태를 전송한다.**

### 생성자 - 접속시 방목록 띄우기

```java
		System.out.println("방목록 관리 : "+cs.roomList.size());
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
		this.broadCasting(msg);//내가 입력한 메세지를 보내주는 것
	} catch (Exception e) {
		System.out.println(e.toString());
	}
}////////////////////////////end of TalkServerThread////////////////////////////
```

* 1번 : ChatServer의 roomList에 값이 제대로 담겼는지 확인하는 단위테스트문장.
* 4번 : 생성된 그룹방이 하나라도 있다면
* 5번 : Room클래스를 인스턴스화 하여 인스턴스 변수에 roomList의 i번째 방정보를 담는다. -반복
* 6번 : i번째 방의 이름을 담는 변수를 선언, 생성한다. - 반복
* 7번 : 멤버변수 g\_title을 가져온 방의 이름으로 초기화한다. - 반복
* 8번 : 변수를 하나 생성해서 기본 인원수를 0으로 초기화한다. -반복\
  \- 값이 이상해지지않도록 현재인원수를 꺼내기전에 계속 초기화해야한다.
* 9번 : room.userList는 접속자정보가 담긴 Vector이므로 접속자가 있는경우 실행되는 if문이다. -반복
* 10번 : 접속자가 있다면 current변수에 해당 방의 유저정보의 방 갯수를 담는다. -반복\
  \- 화면에 표시할 현재 입장인원 담기, 반복될때마다 새로운방의 새로운 인원정보를 담는다.
* 12번 : 멤버변수 g\_current에 가져온 현재 인원을 담는다. - 반복
* 13번 : '나'에게 생성되어있는 방 이름, 현재 인원수를 전송한다. - 반복\
  \- **'나'가 접속시** '**나'의 화면에 현재 존재하는 모든 그룹방을 표시해주기**

### roomCasting메서드 - 해당방에만 전송

```java
//해당 톡방에 있는 사람들에게만 전송
	public void roomCasting(String msg, String roomTitle) {
		for(int i=0;i<cs.roomList.size();i++) {
			Room room = cs.roomList.get(i); 

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

* 한 그룹방에 있는 접속자들에게만 말하는 메서드를 만들어보자
* 3번 : 우선, 맞는 방에 전송해야 할 것이다. 전체 방목록을 가져와 비교한다.\
  \- ChatServer의 roomList.size만큼 반복한다.
* 4번 : Room클래스를 인스턴스화하고, 인스턴스 변수에 ChatServer의 roomList Vector의 i번째 방의 정보를 담는다.
* 6번 : 꺼내온 i번째 방이름과 파라미터로 받아온 방이름을 비교한다.-반복\
  \- 값이 확실한 파라미터를 앞에 써서 비교해야 nullPoint가 떨어지지 않는다.\
  \- 웹에서 멤버변수는 Private로 선언하므로 만들어둔 Room클래스의 getter 메서드를이용해 값을 받아와야한다.
* 7번 : 방이름이 같다면 이제 모든 접속자에게 보내기위한 반복문이 필요하다. \
  \- Room클래스에서 userList의 size만큼 반복한다.
* 8번 : ChatServerThread의 인스턴스변수에 userList의 i번째 방 정보를 담는다.\
  \- **userList는 nickname같은 값이 아닌 thread를 담는 Vector이므로 ChatServerThread타입에 담는다.**
* 10번 : thread에 전송\
  \- 방이름이 같은 모든 접속자 thread에게 말한다.

### run메서드 - case 방 생성

```java
	public void run() {//ChatServer에서 thread가 배분된 다음 호출된다.
		String msg = null;
		boolean isStop = false;
		try {
			run_start:///while문을 탈출하는 label
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
				switch(protocol) {
				
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

* 18번 : ROOM\_CREATE구분번호는 방생성시 화면에 방이름과 현재 인원을 띄우기 위함이다.
* 19,20번 : 이를 위해 방이름과 현재인원을 담는 변수를 선언, 생성한다.
* 22번 : 생성된 방을 **그룹방관리 클래스인 Room클래스에 반영**해야한다.\
  \- 만들어둔 Room클래스의 생성자 중에 파라미터로 방이름, 인원수를 갖는 생성자를 호출한다.
* 24번 : 방이 생성되면 **ChatServer의 Vector인 roomList에 추가**해야한다.
* 26번 : '내'가 생성한 방 정보에 대한 알림을 대기실의 모든 접속자에게 전송한다.\
  \- 대기실의 모든 접속자들의 화면에 해당 방이 보여져야한다.

## ChatClientThreadVer2

```java
public class ChatClientThreadVer2 extends Thread {

	ChatClientVer2 ccv2 = null;

	public ChatClientThreadVer2(ChatClientVer2 ccv2) {
		this.ccv2 = ccv2;
	}
```

* 새로만든 ChatClientVer2를 새로 객체주입을 통해 인스턴스화한다.

### run메서드 - 공통부분

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
					protocol = Integer.parseInt(st.nextToken());//번호를 선언해둔 변수에 담는다
				}
				switch(protocol) {
```

* 11번 : nextToken으로 꺼낸 값은 String 타입이므로 Integer클래스의 parseInt함수로 형전환한다.

### run메서드 - Protocol.WAIT

```java
				case Protocol.WAIT:{
					String nickName = st.nextToken();
					String state = st.nextToken();//대기 또는 방이름이 담기는 (상태)변수
					Vector<String> v_nick = new Vector<>();
					v_nick.add(nickName);
					v_nick.add(state);
					ccv2.wr.dtm_wait.addRow(v_nick);
					//테이블 목록이 늘어날 때 자동으로 스크롤바 이동하기 구현
					ccv2.wr.jsp_wait.getVerticalScrollBar()
					                    .addAdjustmentListener(new AdjustmentListener() {
						@Override
						public void adjustmentValueChanged(AdjustmentEvent e) {
							JScrollBar jsb = (JScrollBar)e.getSource();
							jsb.setValue(jsb.getMaximum());
						}						
					});
				}break;
```

* Protocol.WAIT의 경우에는 닉네임과 현재 위치를 듣게된다.
* 4번 : 화면의 JTable의 dtm에 값을 담아야(addRow)해야 하므로 Vector를 선언, 상생한다.\
  \- String타입으로 한다.\
  \- 테이블에 컬럼이 두개이므로 Vector를 사용한다.
* 5,6번 : 들은 값을 Vector에 담는다.
* 7번 : JFrame이 있는 ChatClientVer2클래스의 dtm\_wait에 addRow한다.
* 9번 : jsp\_wait라는 JTable의 JScrollBar클래스의 세로스크롤바의 값을 읽어온다.\
  \- 소유주.getVerticalScrollBar( )
* 10번 : 읽어온 스크롤바에 AdjustmentListener인터페이스를 add하고 바로 추상메서드를 구현한다.\
  \- 소유주.addAdjustmentListener(new AdjustmentListener( ) {사용 메서드 오버라이드}
* 12번 : 읽어온 스크롤바의 위치값이 변경되는 이벤트에 대한 구현문이다.\
  \- adjustmentValueChanged( )
* 13번 : JScrollBar의 인스턴스 변수를 이벤트 source로 생성하는, 캐스팅연산자로 타입을 맞춰야한다.
* 14번 : 생성한 스크롤바의 위치를 set한다. 최대위치로\
  \- 소유주.getValue(소유주.getMaximum( ));

### run메서드 - Protocol.ROOM\_CREATE

```java
				case Protocol.ROOM_CREATAE:{
					JOptionPane.showMessageDialog(ccv2, "Client ROOM_CREATE");
					String roomTitle = st.nextToken();
					String currentNum = st.nextToken();
					Vector<String> v_room = new Vector<>();
					v_room.add(roomTitle);
					v_room.add(currentNum);
					ccv2.wr.dtm_room.addRow(v_room);
					ccv2.wr.jsp_room.getVerticalScrollBar()
															.addAdjustmentListener(new AdjustmentListener() {
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
	}/////////////////////////////////end of run///////////////////////////////////
```

* Protocol.ROOM\_CREATE의 경우에는 생성된 방제목과 현재 인원수 정보를 듣게된다.
* 5번 : dtm에 두개 컬럼의 값을 addRow하기 위해 String타입 Vector를 선언, 생성한다.
* 6-8번 : Vector에 값을 담아 ChatClientVer2의 dtm\_room에 addRow한다.
* 9-16번 : 테이블 증가에 따른 스크롤 자동이동 처리
