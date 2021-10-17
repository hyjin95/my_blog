# 톡방 생성&입장 구현 마무리

## 그룹방 생성

##  ChatServerThread

### 생성자

```java
public ChatServerThread(ChatServer cs) {
    this.cs = cs;//부모의 정보를 갖게된다.
		this.client = cs.client;//초기화 누락시:NullPointException = 소켓 동기화
		try {
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
```

* 대기실에 입장과 동시에 대기실의 모든 접속자 정보와 그룹방 정보를 띄워준다.

## ChatClientThreadVer2

### 선언부

```java
public class ChatClientThread extends Thread {
	
	String path = "C:\\workspace_java\\dev_java\\src\\net\\tomato_step4\\"; 
	
	ChatClientVer2 ccv2 = null;

	public ChatClientThread(ChatClientVer2 ccv2) {
		this.ccv2 = ccv2;
	}
```

### run메서드

```java
public void run() {
    switch(protocol) {
    case Protocol.ROOM_LIST:{
					JOptionPane.showMessageDialog(ccv2, "Client ROOM_LIST");
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
```

* 그룹대화방이 생성되면, 대기실의 모든 접속자들의 화면에도 갱신되어야한다.
* 이를 위해 ChatClientThreadVer2에 run-case Protocol.ROOM_LIST를 추가한다.
* serverThread에서 듣는 정보 : 방이름, 현재인원

## 그룹방 입장

## ChatServerThread

### 선언부

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
	String nickName = null;
```

### run메서드 - 1 

```java
public void run() {
    switch(protocol) {
    //////////////////////////////방에 입장하기////////////////////////////////
		case Protocol.ROOM_IN:{
					//클라이언트에서 전송된 메세지 읽어오기
					String roomTitle = st.nextToken();
					String nickName = st.nextToken();
					//대기실 대화방 목록의 해당 대화방 인원수 업데이트--1
					for(int i=0;i<cs.roomList.size();i++) {
						Room room = cs.roomList.get(i);
						if(roomTitle.equals(room.title)) {
							g_title = roomTitle;
							g_current = room.current+1;
							//아래 코드가 없으면 인원수 업데이트가 일어나지 않는다.
							room.setCurrent(g_current);
							//해당 방의 userList에 '나'를 추가 해야한다.--2
							room.userList.add(this);
							//해당 방의 nameList에 입장한 사람의 닉네임을 추가해야한다.--3
							room.nameList.add(nickName);
						}
					}			
					broadCasting(Protocol.ROOM_IN+Protocol.seperator+g_title
								    	 		+Protocol.seperator+g_current
								    	 		+Protocol.seperator+this.nickName);//--4
```

* 기존 코드

### run메서드 - 2

```java
public void run() {
    switch(protocol) {
    //////////////////////////////방에 입장하기////////////////////////////////
		case Protocol.ROOM_IN:{
					//클라이언트에서 전송된 메세지 읽어오기
					String roomTitle = st.nextToken();
					String nickName = st.nextToken();
						
					//그룹방 접속자 목록 갱신처리 추가,ROOM_INLIST, MessageRoom.java소스에 반영
					for(int i=0;i<cs.roomList.size();i++) {//방갯수 만큼 for문
						Room room = cs.roomList.get(i);
						String title = room.title;
						g_title = title;
						int current = 0;
						if(room.userList!=null && room.userList.size()!=0) {//정원 수 갱신
							current = room.userList.size();
						}
						for(int j=0;j<room.nameList.size();j++) {//ROOM_INLIST반영
							if(!nickName.equals(room.nameList.get(j))) {
								if(roomTitle.equals(room.title)) {//방 이름이 같은 경우에만 전송처리
									ChatServerThread cst = room.userList.get(j);
									cst.send(Protocol.ROOM_INLIST+Protocol.seperator+g_title
								    	 		+Protocol.seperator+g_current
								    	 		+Protocol.seperator+nickName);
								}
							}
						}
					}
				}break;
				////////////////////////////end of 방입장///////////////////////////////
				broadCasting(Protocol.ROOM_IN+Protocol.seperator+g_title
								    	 		+Protocol.seperator+g_current
								    	 		+Protocol.seperator+this.nickName);//--4
		}break;
```

* WaitRoom클래스의 화면에서 입장 이벤트가 발생하면 말하는 내용을 처리하는 부분\
  \- Protocol.ROOM_IN로 듣기 = 방입장
* MessageRoom클래스의 화면 중 톡방 참가자 목록이 갱신되어야한다.\
  \- Protocol.ROOM_INLIST로 말하기 = 방에 입장한 인원 목록
* 10번 : 방이름이 같은지 비교해야하므로 ChatServer의 모든 방갯수 만큼 for문을 돌린다.
* 11번 : Room 인스턴스변수로 roomList의 i번째 방의 정보를 꺼낸다.
* 12,13번 : i번째 방의 이름을 꺼내 멤버변수에 초기화한다.
* 15번 : 해당방의 userList(thread)에 thread가 하나라도 참가중이라면,
* 16번 : 멤버변수 current에 room.userLIst.size( )메서드로 수를 담는다.\
  \- 현재 해당 방에 접속해 있는 thread, 사람의 수
* 18번 : 나를 제외한 접속자들에게 적용해야 하므로, 내가아닌 접속자들 중 방이름이 같은 상태인 접속자인 경우에 클라이언트로 전송한다.\
  \- room의 nameList만큼 만복한다.
* 19번 : '나'와 j번째 이름이 다르다면
* 20번 : 그리고 '나'의 방이름과 j번째 접속자의 방 이름이 같다면,
* 21번 : 멀티스레드 관리자인 cst인스턴스변수에 j번째 thread를 담는다.
* 22-15번 : '내'가 아니고 같은 방에 원래 있던 접속자들이라면 전송한다.\
  \- Protocol.ROOM_INLIST+방이름+현재인원+닉네임

## ChatClientThreadVer2

### 선언부

```java
public class ChatClientThreadVer2 extends Thread {
	
	String path = "C:\\workspace_java\\dev_java\\src\\net\\tomato_step4\\"; 
	
	ChatClientVer2 ccv2 = null;

	public ChatClientThreadVer2() {}

	
	public ChatClientThreadVer2(ChatClientVer2 ccv2) {
		this.ccv2 = ccv2;
	}
```

### run메서드 - ROOM_INLIST

```java
public void run() {
     switch(protocol) {
     case Protocol.ROOM_INLIST:{
					String roomTitle = st.nextToken();
					String currentNum = st.nextToken();
					String nickName = st.nextToken();
					Vector<String> v_room = new Vector<>();
					v_room.add(nickName);
					ccv2.mr.dtm.addRow(v_room);
		 }break;
```

* 해당방에 입장해있는 '나'를 제외한 모든 접속자에게 해당하는 구문
* 해당 접속자들의 화면 dtm에 입장한 사람의 정보를 addRow한다.

### run메서드 - ROOM_IN

```java
				////////////////////////////ROOM_IN///////////////////////////////////
				//220|방이름|현재인원수|내닉네임|홍길동#이순신#김춘추#....
				case Protocol.ROOM_IN:{
					String roomTitle = st.nextToken();
					String current   = st.nextToken();
					String nickName  = st.nextToken();
					String names[] = new String[Integer.parseInt(current)];
					StringTokenizer st_name = null;
```

* serverThread에서 넘어온 전송값이 Protocol.ROOM_IN이라면 처리되는 구문
* 7번 : 모든 접속자들의 닉네임을 담기위한 String배열 선언, 생성\
  \- 방이 접속인원만큼 있어야 하므로 Object인 current값을 Interger.parseInt로 형전환하여 배열 크기로한다.

```java
			//입장이 성공하면 대기실 table에 보여지는 방인원수를 갱신해야한다.
			for(int i=0;i<ccv2.wr.jtb_room.getRowCount();i++) {//해당 방 인원 갱신
				if(roomTitle.equals((String)ccv2.wr.dtm_room.getValueAt(i, 0))) {
					ccv2.wr.dtm_room.setValueAt(current, i, 1);
					break;
				}
			}
```

* 입장이 성공하면 대기실 table에 보여지는 방인원수를 갱신한다.
* 2번 : 해당 방의 인원갱신을 위해 갱신할 JTable의 모든 row와 비교해서 방을 찾는다.\
  \- rowCount는 jtb에서, 값 비교는 dtm에서 해야한다.
* 3번 : 전송된 방 이름과 table의 i번째 row의 방 이름이 같다면,\
  \- dtm에서 방 이름은 0번째 컬럼이다.\
  \- getValueAt( )으로 꺼낸 값은 Obejct타입이므로 문자 비교를 위해 String캐스팅 연산자로 형전환한다.
* 4번 : JTable에서 전송된 방 이름과 같은 이름을 가진 row의 인원수 data를 새로 초기화한다.\
  \- dtm.setValueAt(전송된 current, i번째 row, 1번째 컬럼);\
  \- table에서 인원수 정보가 담긴 곳은 1번째 컬럼이다.

```java
			//입장이 성공하면 대기실 table에 보여지는 해당 참가자의 위치를 방 이름으로 갱신
			for(int i=0;i<ccv2.wr.jtb_wait.getRowCount();i++) {
			  if(nickName.equals((String)ccv2.wr.dtm_wait.getValueAt(i, 0))){
			    ccv2.wr.dtm_wait.setValueAt(roomTitle, i, 1);
			    break;
        }
    	}
```

* 입장이 성공하면 대기실 table의 해당 참가자의 위치를 방 이름으로 갱신한다.
* 대기실 입장인원 목록을 표시하는 JTable의 row와 비교해야한다.
* 2번 : 대기실 입장목록의 모든 row와 비교해야한다. getRowCount( )메서드로 반복 수 결정
* 3번 : 반복하는 중에 전송된 nickName값과 문자열이 같은 정보가 있다면
* 4번 : table의 i번째 row, 1번째 컬럼의 값을 전송된 방 이름값으로 초기화한다.

```java
			//입장(waitRoom.java)를 누른 사람만 화면을 이동(MessageRoom.java) 처리
			if(ccv2.nickName.equals(nickName)) {
				ccv2.jtp.setSelectedIndex(1);
				ccv2.mr.jtf_client.requestFocus();
				
				for(int x=0;x<ccv2.wr.jtb_wait.getRowCount();x++) {
					if(roomTitle.equals(ccv2.wr.dtm_wait.getValueAt(x, 1))) {
						String imsi[] = {(String)ccv2.wr.dtm_wait.getValueAt(x, 0)};
						ccv2.mr.dtm.addRow(imsi);
					}
				}
			}
		}break;
		////////////////////////////ROOM_IN///////////////////////////////////
```

* 입장한 사람의 화면을 대기실 tab에서 대화방 tab으로 자동이동시켜준다.\
  \- waitRoom.java -> MessageRoom.java
* 2번 : 화면의 nickName과 서버에서 전송된 nickName이 같다면,
* 3번 : 해당 nickName사용자의 화면을 1번 tab으로 전환한다.\
  \- JTabbedPane클래스에 붙인 0번째 tab = waitRoom\
  \- JTabbedPane클래스에 붙인 1번째 tab = MessageRoom
* 4번 : requestFocus( )함수를 이용해 커서를 자동으로 이동시켜준다.\
  \- 커서를 이동시키지 않으면 입장한 뒤에 메세지를 입력하려면 입력창을 한번 클릭해야하는 번거로움이 있다.
* 입장과 화면전환이 성공적으로 이루어지면 대화방 안의 인원목록에 입장한 사람을 추가, 갱신한다.
* 6번 : 대기실 대기인원 목록의 row 수만큼 for문을 돌려 비교값을 찾는다.
* 7번 : 그 중에 상태, 위치(x번째 로우, 1번째 컬럼)가 서버에서 받은 방이름과 같은 row가 있다면,
* 8번 : String 배열에 해당 row의 0번째 컬럼값인 nickName을 받아와 
* 9번 : 대화방의 입장한 사람의 화면 dtm에 add한다.\
  \- 입장한 사람의 화면에 해당 방의 모든 접속자를 표시한다.

