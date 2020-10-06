# 대화방 입장 이벤트 구현

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

### actionPerformed - 입장 버튼

```java
@Override
	public void actionPerformed(ActionEvent ae) {
		Object obj = ae.getSource();
		else if(obj==jbtn_in) {
			//어느 방으로 갈거니?
			int index[] = jtb_room.getSelectedRows();
			if(index.length==0) {//방을 선택하지 않았을때
				//파라미터 : 부모, 메세지--panel은 부모가 될수 없으므로 ChatClient2클래스를 사용
				JOptionPane.showMessageDialog(ccv2, "입장할 방을 선택하세요."
																			, "Error", JOptionPane.ERROR_MESSAGE);
				return;
			}else {//방을 선택했을 때
				try {//입장 전달
					for(int i=0;i<jtb_room.getRowCount();i++) {
						if(jtb_room.isRowSelected(i)) {
							String roomTitle = (String)dtm_room.getValueAt(i, 0);//Object 타입
							ccv2.oos.writeObject(Protocol.ROOM_IN+Protocol.seperator+roomTitle
																 +Protocol.seperator+ccv2.nickName);
						}
					}
				} catch (Exception e) {
					e.printStackTrace();
				}
			}
			jtb_room.clearSelection();//선택한 로우에 대한 초기화
		}////////////////////////jbtn_in 입장 버튼 클릭시//////////////////////////////
```

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

### run메서드 

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
				///////////////////////////방에 입장하기///////////////////////////////////
				case Protocol.ROOM_IN:{
					//클라이언트에서 전송된 메세지 읽어오기
					String roomTitle = st.nextToken();
					String nickName = st.nextToken();
					//대기실 대화방 목록의 해당 대화방 인원수 업데이트--1
					for(int i=0;i<cs.roomList.size();i++) {
						Room room = cs.roomList.get(i);
						if(roomTitle.equals(room.getTitle())) {
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
				}break;
				////////////////////////////end of 방입장///////////////////////////////
```

## ChatClientThread

* 37일차에 구

