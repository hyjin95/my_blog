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

* 대화방 입장에 필요한 정보 : 닉네임, 방이름, 인원수
* 방 관리 클래스인 Room클래스의 인스턴스변수도 선언한다.

### actionPerformed - 입장 버튼

```java
@Override
	public void actionPerformed(ActionEvent ae) {
		Object obj = ae.getSource();
		else if(obj==jbtn_in) {
			//어느 방으로 갈거니?
			int index[] = jtb_room.getSelectedRows();
			if(index.length==0) {//방을 선택하지 않았을때
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

* 대기실에서 입장버튼 클릭시 이벤트가 발생한다.
* 6번 : 방목록의 선택은이 JTable서 인지하므로 getSelectedRow( )함수를 이용해 int배열에 담는다.\
  \- 배열에 담는 이유는 방 선택값이 없는 경우를 정의하기 위함이다.
* 7-10번 : 방을 선택하지 않은경우에 알림창을 띄운다.\
  \- showMessageDialog(부모, 메세지, 알림창title, 알림타입);\
  \- Panel은 자체로 존재할수 없으므로 JFrame을 상속받는 부모클래스 인스턴스변수를 파라미터로 한다.\
  \- 더 진행하지 못하므로 반드시 return;을 넣어 탈출시킨다. 
* 11,12번 : 방이 선택되면 입장을 server에 전송해야하므로 예외처리 구문안에서 처리한다.
* 13,14번 : JTable의 방목록중에서 i번째 방을 선택했다면,\
  \- JTable의 방목록을 getRowCount( )메서드와 for문으로 돌려 선택을 확인한다. \
  \- JTable의 목록이 소유주이고, isRowSelected(i)메서드를 이용해 i번째 방이 선택되면 진행한다.
* 15번 : 선택한 JTable의 로우는 i번째 이고, 값은 dtm에 들어있다.\
  \- 해당 JTable의 dtm.getValueAt메서드로 i번째 로우의 0번째 컬럼 값을 String타입 변수에 담는다.\
  \- getValueAt메서드로 꺼낸 값은 Object타입이므로 캐스팅연산자로 형전환한다.
* 16번 : WaitRoom에는 소켓이 없으므로 ChatClientVer2클래스에 구현된 oos를 통해 말한다.\
  \- Protocol.ROOM_IN+선택한 방이름+'나'의 nickName (ccv2.nickName)

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

* 대화방 입장에 대해 필요한 정보인 현재인원수를 담을 int변수, 현재 방 이름을 담을 String변수, '나'의 닉네임을 담을 String변수가 필요하다.

### run메서드 

```java
	public void run() {//ChatServer에서 thread가 배분된 다음 호출된다.
				switch(protocol) {
				///////////////////////////방에 입장하기////////////////////////////////
				case Protocol.ROOM_IN:{
					//클라이언트에서 전송된 메세지 읽어오기
					String roomTitle = st.nextToken();
					String nickName = st.nextToken();
					for(int i=0;i<cs.roomList.size();i++) {
						Room room = cs.roomList.get(i);
						if(roomTitle.equals(room.getTitle())) {
							g_title = roomTitle;
							g_current = room.current+1;
							//아래 코드가 없으면 인원수 업데이트가 일어나지 않는다.
							room.setCurrent(g_current);
							//해당 방의 userList에 '나'를 추가--2
							room.userList.add(this);
							//해당 방의 nameList에 입장한 닉네임 추가--3
							room.nameList.add(nickName);
						}
					}					
					broadCasting(Protocol.ROOM_IN+Protocol.seperator+g_title
								    	 		+Protocol.seperator+g_current
								    	 		+Protocol.seperator+this.nickName);//--4
				}break;
				//////////////////////////end of 방입장//////////////////////////////
```

* Protocol.ROOM_IN의 경우에 듣는것은 입장할 방이름과 입장할 nickName이다.
* 일단 **대기실의 대화방 목록의 현재 인원수를 업데이트**해야한다.
* 8번 : 같은방인지 확인해야하므로 ChatServer가 관리하는 roomList의 size만큼 for문을 돌린다.
* 9번 : 방관리 클래스 Room의 인스턴스변수를 roomList의 i번째 방으로 생성한다.-반복
* 10번 : 읽어온 방이름과 roomList의 i번째 방의 title이 같다면,\
  \- 아니라면 for문을 반복한다.
* 11번 : 멤버변수 g_title을 방이름으로 초기화한다.
* 12번 : 멤버변수 g_current를 room.current에 +1하여 초기화한다.\
  \- room.current : 해당 방의 원래 인원수
* 14-18번 : i번째 방의 변수들을 업데이트해야한다.\
  \- **인원수를 업데이트**하고, userList에 **입장한 thread(this)를 추가**하고, nameList에 **닉네임을 추가**한다.
* 21번 : 방이름, 방인원수, 입장한 사람의 정보를 말한다.\
  \- Protocol.ROOM_IN+g_title+g_current+this.nickName

## ChatClientThread

* 37일차에 구현
