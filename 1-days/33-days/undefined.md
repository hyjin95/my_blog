# 대화명 변경 구현

## TomatoClient

### 선언부

```java
public class TomatoClient extends JFrame implements ActionListener{
	String nickName = null;
	JButton jbtn_change = new JButton("대화명 변경");
```

### 화면

```java
public void initDisplay() {
    jbtn_change.addActionListener(this);
}    
```

### actionPerformed

```java
public void actionPerformed(ActionEvent e) {
   if (obj==jbtn_change) {
			//변경할 대화명 입력받기
			String afterName = JOptionPane.showInputDialog("변경할 대화명을 입력하세요.");
			//대화명이 null이거나 문자열의 길이가 얼마인지 체크하기 -유효성 체크
			if(afterName.equals(this.nickName)) {
				JOptionPane.showMessageDialog(this, "기존 대화명과 같습니다. 다른 대화명을 입력하세요."
												  ,"info",JOptionPane.INFORMATION_MESSAGE);
				return;
			}
			else if(afterName==null||afterName.trim().length()<1) {
				JOptionPane.showMessageDialog(this, "변경할 대화명은 최소 한자 이상이여야 합니다."
						 						  ,"info",JOptionPane.INFORMATION_MESSAGE);
				return;
			}
			//대화명이 변경된 사실을 광고하기
			else {
				try {
					oos.writeObject(Protocol.CHANGE+"#"+nickName+"#"+afterName+"#"
									+"["+nickName+"]"+"님의 대화명이 "+"["+afterName+"]"+"으로 변경되었습니다.");
				} catch (Exception e2) {
					e2.printStackTrace();
				}
			}
		}
}
```

* JOptionPane.showInputDialog\( \); 함수를 이용해 입력화면을 띄운다.
* 11번 : trim 함수를 이용해 글자 자릿수를 체크한다.

## TomatoServerThread

### 선언부

```java
public class TomatoServerThread extends Thread {
	TomatoServer ts = null;
	Socket client = null;
	ObjectOutputStream oos = null;
	ObjectInputStream  ois = null
```

### run메서드

```java
public void run() {
String msg = null;
		boolean isStop = false;
		try {
			run_start://while문을 탈출하는 label을 만든다.
			while(!isStop) {
				msg = (String)ois.readObject();
				ts.jta_log.append(msg+"\n");
				ts.jta_log.setCaretPosition(ts.jta_log.getDocument().getLength());
				StringTokenizer st = null;
				int protocol = 0;
				if(msg!=null) {//메세지가 있다면
					st = new StringTokenizer(msg,"#");
					protocol = Integer.parseInt(st.nextToken());					
				}
				switch(protocol) {
				case Protocol.CHANGE:{//대화명 변경
					String nickName = st.nextToken();
					String afterName = st.nextToken();
					String message = st.nextToken();
				    this.nickName = afterName;
				    this.broadCasting(Protocol.CHANGE+"#"+nickName+"#"+afterName+"#"+ message);
				}break;
}
```

* 22번 : 대화명 변경알림은 변경자와 현재 채팅방의 접속자 모두에게 말해야한다. - broadCasting메서드를 사용

## TomatoClientThread

### 선언부

```java
public class ChatClientThread extends Thread {	
	ChatClient cc = null;
	public ChatClientThread(ChatClient cc) {
		this.cc = cc;
	}
```

### run메서드

```java
public void run() {
		boolean isStop = false;
		while(!isStop) {
			try {
				String msg = "";
				msg = (String)cc.ois.readObject();
				StringTokenizer st = null;//해당사항이 없을 수 있어 생성은 따로한다.
				int protocol = 0;
				if(msg!=null) {
					st = new StringTokenizer(msg,"#");
					protocol = Integer.parseInt(st.nextToken());//번호를 선언해둔 변수에 담는다.
				}///end of if
				switch(protocol) {
				case Protocol.CHANGE:{//대화명 변경
					String nickName = st.nextToken();
					String afterName = st.nextToken();
					String message = st.nextToken();
					//테이블에 출력된 대화명도 바꿔준다.
					for(int i=0;i<tc.dtm.getRowCount();i++) {
						//대화명 변경전에 현재 테이블에서 가져온 대화명을 받는다.
						String cname = (String)tc.dtm.getValueAt(i, 0);
						if(cname.equals(nickName)) {
							tc.dtm.setValueAt(afterName, i, 0);
							break;
						}
					}
					tc.jta_display.append(message+"\n");
					tc.jta_display.setCaretPosition(tc.jta_display.getDocument().getLength());
					//채팅창의 타이틀 변경
					if(nickName.equals(tc.nickName)) {
						tc.setTitle(afterName+"님의 대화창");
						tc.nickName = afterName;
					}
				}break;
```

* 19-24번 : 현재 접속자 목록을 보여주는 JTabld의 dtm data를 변경시켜야한다. - 19번 : for문을 이용해 dtm의 모든 row와 비교한다. - 21번 : String타입 변수에 dtm에서 가져온 data를 담는다.   getVlaueAt\(모든row, 첫번째 컬럼\) - 22번 : 가져온 data와 기존data가 같다면 새로 설정된 after데이터를 set한다.   setValueAt\(변경된 대화명, 해당 row, 첫번째컬럼\)
* 27번 : 현재 모든 접속자에게 메세지를 출력한다.
* client의 채팅방 title을 변경시켜야한다. - 30번 : 변경전 대화명이 tomatoClient의 멤버변수 nickName과 같다면 - 31번 : tamatoClient의 title을 변경된 대화명으로 set해주고, - 32번 : tomaroClient의 멤버변수 nickName도 변경된 대화명으로 초기화한다.

