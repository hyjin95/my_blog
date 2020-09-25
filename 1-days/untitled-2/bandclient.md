# BandClient

역할 : 접속자에게 대화창을 제공해주고, 입력값을 socket에 담아주는 클래스

## BandClient.java

선언부, init, 화면구현, main메서드 삭제,

{% page-ref page="../30-days/talkclient.md" %}

### 선언부+

```java
package net.tomato_step2;

public class BandClient extends JFrame implements ActionListener{
	Socket socket = null;//소켓2
	ObjectInputStream  ois = null;//듣기
	ObjectOutputStream oos = null;//말하기
	
	JPanel     jp_second       = new JPanel();
	JPanel     jp_second_south = new JPanel();
	String     cols[]          = {"대화명"};
	String     data[][]        = new String [0][1];
	DefaultTableModel dtm      = new DefaultTableModel(data, cols);
	JTable     jtb         = new JTable(dtm);
	JScrollPane jsp_nick = new JScrollPane(jtb, JScrollPane.VERTICAL_SCROLLBAR_AS_NEEDED
			   							      , JScrollPane.HORIZONTAL_SCROLLBAR_AS_NEEDED);
	//닉네임 입력받아서 담기
	String nickName = null;
```

### 생성자

```java
	public BandClient() {}
	
	public BandClient(String nickName) {
		this.nickName = nickName;
	}
```

### init+ - ClientThread호출

```java
 public void init() {
		try {
			//소켓 인스턴스화
			socket = new Socket("192.168.0.128",3007);//소켓2	
			oos = new ObjectOutputStream(socket.getOutputStream());//소켓2 말하기구현
			ois = new ObjectInputStream(socket.getInputStream());//소켓2 듣기 구현
			//서버에게 내가 입장한 사실을 알린다.말하기
			oos.writeObject(100+"#"+nickName);
			//서버의 말을 듣기 위한 준비
			BandClientThread tct = new BandClientThread(this);
			tct.start();
		} catch (Exception e) {
			System.out.println(e.getMessage());
		}		
	}///////////////////////////////end of init///////////////////////////////////
```

### actionPerformed - 버튼

```java
	@Override
	public void actionPerformed(ActionEvent e) {
		Object obj = e.getSource();
		String msg = jtf_msg.getText();
		if(obj==jbtn_send || obj == jtf_msg) {
			try {
				oos.writeObject(200+"#"+nickName+"#"+msg);
				jtf_msg.setText("");
			} catch (Exception e2) {
				System.out.println(e2.toString());
			}			
		}//////////////jbtn_send, jtf_msg///////////////////
		else if(obj==jbtn_close) {
			try {
				oos.writeObject(500+"#"+nickName);
				System.exit(0);
			} catch (Exception e2) {
				System.out.println(e2.toString());
			}			
		}//////////////////jbtn_close////////////////////////
		
	}/////////////////////////////end of actionPerformed//////////////////////////////////
}

```

