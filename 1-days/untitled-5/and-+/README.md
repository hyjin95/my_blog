# 톡방 생성&입장 구현 마무리

## 그룹방 생성

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
	String nickName = null;;
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
						
					//그룹방 접속자 목록 갱신처리 추가 - ROOM_INLIST - MessageRoom.java소스에 반영
					for(int i=0;i<cs.roomList.size();i++) {//방갯수 만큼 for문을 돌려야한다.
						Room room = cs.roomList.get(i);
						String title = room.title;
						g_title = title;
						int current = 0;
						if(room.userList!=null && room.userList.size()!=0) {//정원 수 갱신
							current = room.userList.size();//현재 그 방에 접속해있는 사람의 수
						}
						for(int j=0;j<room.nameList.size();j++) {//ROOM_INLIST반영
							//내가 아닌 다른 이름 중 방이름이 같은 경우만 클라이언트로 전송
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
```

### ChatClientThreadVer2

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

### run메서드 - ROOM\_INLIST

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

### run메서드 - ROOM\_IN

```java
				/////////////////////////////////ROOM_IN////////////////////////////////////////
				//220|방이름|현재인원수|내닉네임|홍길동#이순신#김춘추#....
				case Protocol.ROOM_IN:{
					String roomTitle = st.nextToken();
					String current   = st.nextToken();
					String nickName  = st.nextToken();
					String names[] = new String[Integer.parseInt(current)];
					StringTokenizer st_name = null;
					
					//입장이 성공하면 대기실 table에 보여지는 방인원수를 갱신해야한다.
					for(int i=0;i<ccv2.wr.jtb_room.getRowCount();i++) {//해당 방의 인원을 갱신해야한다.
						//값비교는 dtm에서, rowCount는 jtb에서 해야한다. 방이름 비교이므로 dtm의 0번째 컬럼이다.
						if(roomTitle.equals((String)ccv2.wr.dtm_room.getValueAt(i, 0))) {
							ccv2.wr.dtm_room.setValueAt(current, i, 1);
							break;
						}
					}
					//입장이 성공하면 대기실 table에 보여지는 해당 참가자의 위치를 방 이름으로 갱신해야한다.
					for(int i=0;i<ccv2.wr.jtb_wait.getRowCount();i++) {
						if(nickName.equals((String)ccv2.wr.dtm_wait.getValueAt(i, 0))){
							ccv2.wr.dtm_wait.setValueAt(roomTitle, i, 1);
							break;
						}
					}
					//입장(waitRoom.java)를 누른 사람만 화면을 이동(MessageRoom.java) 처리
					if(ccv2.nickName.equals(nickName)) {//화면 nickName과 서버에서 들어온 nickName이 같니?
						ccv2.jtp.setSelectedIndex(1);//텝0번=대기실 텝1번=대화방	
						ccv2.mr.jtf_client.requestFocus();//커서를 자동으로 이동시켜주는 메서드 대화방 입장시 입력창을 클릭해도되지 않게 하려고
						for(int x=0;x<ccv2.wr.jtb_wait.getRowCount();x++) {
							if(roomTitle.equals(ccv2.wr.dtm_wait.getValueAt(x, 1))) {
								String imsi[] = {(String)ccv2.wr.dtm_wait.getValueAt(x, 0)};
								ccv2.mr.dtm.addRow(imsi);
							}
						}
					}
				}break;
				/////////////////////////////////ROOM_IN////////////////////////////////////////
```

