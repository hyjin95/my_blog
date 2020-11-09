# 글자 스타일 변경 구현

## ChatClient

### 선언부

```java
public class ChatClient extends JFrame implements ActionListener{
	Socket socket = null;//소켓2
	ObjectInputStream  ois = null;//듣기
	ObjectOutputStream oos = null;//말하기
	
	//글자색 부분변경, 배경변경, 이모티콘추가, 글씨도 그린다.-JTextPane
	StyledDocument sd_display = new DefaultStyledDocument(new StyleContext());
	JTextPane jtp_display = new JTextPane(sd_display);
	JScrollPane jsp_display = new JScrollPane(jtp_display, JScrollPane.VERTICAL_SCROLLBAR_AS_NEEDED

	JButton    jbtn_style      = new JButton("폰트 스타일");
	JButton    jbtn_fcolor     = new JButton("폰트 색상");
	
	JLabel     jlb_size    = new JLabel("폰트크기");
	JTextField jtf_size    = null;
	JButton    jbtn_bold   = null;
	JButton    jbtn_italic = null;
	
	String fontColor = "0";
	String fontStyle = "Font.PLAIN";
  int    fontSize  = 16;
```

* 스타일 지정에 필요한 클래스들을 생성하고, 사용할 스타일의 기본값을 정한다.

### 화면구현

```java
public void initDisplay() {
		//사용자로부터 닉네입 입력받기		
		//nickName = JOptionPane.showInputDialog("닉네임을 입력하세요.");
		
		this.addWindowListener(new WindowAdapter() {
			public void windowClosing(WindowEvent we) {
				System.exit(0);
			}
		});
		
		//어플리케이션 닫을때 프로세서 종료처리
		this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		
		jbtn_style .addActionListener(this);    
		jbtn_fcolor.addActionListener(this);  
}
```

* this에 WindowListener 인터페이스를 add하는데 WindowAdapter메서드를 통해 생성함으로써  필요한 추상메서드만 재정의하여 사용한다.
* 12번 : this의 JFame 화면이 종료될때 프로세서까지 종료하는 구문\
* 스타일에 필요한 버튼에 event를 붙인다

### actionPerformed-1

```java
@Override
	public void actionPerformed(ActionEvent e) {
		Object obj = e.getSource();
		String msg = jtf_msg.getText();
		if(obj==jbtn_style) {			
			JDialog jdl_style   = new JDialog();//편집창 띄우기
			jtf_size    = new JTextField("12");
			jbtn_bold   = new JButton("굵은글씨");
			jbtn_italic = new JButton("이탈릭");
			
			jtf_size   .addActionListener(this);
			jbtn_bold  .addActionListener(this);
			jbtn_italic.addActionListener(this);
			
			jdl_style.setLayout(new GridLayout(1,4));
			jdl_style.add(jlb_size);
			jdl_style.add(jtf_size);
			jdl_style.add(jbtn_bold);
			jdl_style.add(jbtn_italic);
			
			jdl_style.setTitle("스타일 설정");
			jdl_style.setSize(400, 70);
			jdl_style.setLocation(700, 250);
			jdl_style.setVisible(true);			
		}
		else if(obj==jbtn_bold) {
			fontStyle = "Font.BOLD";
		}
		else if(obj==jbtn_italic) {
			fontStyle = "Font.ITALIC";
		}
		else if(obj==jtf_size) {
			fontSize = Integer.parseInt(jtf_size.getText());
		}/////////////////////////jbtn_style//////////////////////////////////
```

* 스타일버튼 클릭시 JDialog로 설정화면을 띄운다.
* 7-9번 : TextField를 기본값을 넣어서, 버튼을 생성한다.
* 26-33번 : 설정화면에 대한 반응값 - JTextField에 입력되는 text는 String이므로, Integer.parseInt함수를 이용해 형전환한다. 

### actionPerformed-2

```java
		else if(obj==jbtn_fcolor) {
			JDialog jdl_fcolor   = new JDialog();//편집창 띄우기
			JColorChooser jcc = new JColorChooser();
			ColorSelectionModel model = jcc.getSelectionModel();
			//인터페이스 생성과 동시에 구현, 내부클래스, 익명클래스 라고 한다.
			ChangeListener listener = new ChangeListener() {
				@Override
				public void stateChanged(ChangeEvent sc) {
					Color nfgColor = jcc.getColor();//색상정보 읽어오기
					fontColor = String.valueOf(nfgColor.getRGB());//폰트에 입히기
				}				
			};
			model.addChangeListener(listener);
			jdl_fcolor.add(jcc);
			jdl_fcolor.setSize(600, 500);
			jdl_fcolor.setVisible(true);
			
		}/////////////////////////jbtn_fcolor////////////////////////////////
```

* 컬러 버튼 클릭시 JDialog로 설정화면을 띄운다.
* 3번 : JColorChooser클래스로 색상 선택화면을 생성한다.
* 4번 : ColorSelectionModel함수의 변수에 선택된 색상을 담는다.
* 6번 : 변경이벤트를 읽어오는 ChangeListener인터페이스를 생성하면서 메서드를 구현한다.
* 7-11번 : 구현메서드 - 9번 : Color타입 변수에 선택된 색상정보를 담는다. - 10번 : fontColor에 위의 변수의 값을 RGB로 꺼내 String으로 형전환하여 담는다.
* 13번 : 색상모델에 구현메서드를 add한다.

### actionPerformed-3

```java
if(obj==jbtn_send || obj == jtf_msg) {
			try {
				oos.writeObject(Protocol.MULTI+"#"+nickName
							 				  +"#"+msg
							 				  +"#"+fontColor
							 			    +"#"+fontStyle
							 				  +"#"+fontSize);
				jtf_msg.setText("");
			} catch (Exception e2) {
				System.out.println(e2.toString());
			}	
		}////////////////////////jbtn_send, jtf_msg//////////////////////////
```

* 기존 채팅 전달 이벤트 oos에 추가한다.

## ChatServerThread

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
				StringTokenizer st = null;
				int protocol = 0;
				if(msg!=null) {//메세지가 있다면
					st = new StringTokenizer(msg,"#");
					protocol = Integer.parseInt(st.nextToken());					
				}//end of if
				switch(protocol) {
			  case Protocol.MULTI:{//다자간대화
					String nickName  = st.nextToken();
					String message   = st.nextToken();
					String fontColor = st.nextToken();
					String fontStyle = st.nextToken();
					String fontSize  = st.nextToken();
					String imgChoice = "";
					
					broadCasting(Protocol.MULTI+"#"+nickName+"#"+message+"#"+fontColor
											   +"#"+fontStyle+"#"+fontSize);
				}break;
					
```

* 기존의 run메서드의 듣기, 말하기 부분에 추가한다.

## ChatClientThread

### run - 듣기, 화면에 실행하기

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
				case Protocol.MULTI:{//일반 다자간 메세지
					String nickName  = st.nextToken();
					String message   = st.nextToken();
					String fontColor = st.nextToken();
					String fontStyle = st.nextToken();
					String fontSize  = st.nextToken();
					String styles[] = new String[3];
					styles[0] = fontColor;
					styles[1] = fontStyle;
					styles[2] = fontSize;
					SimpleAttributeSet sas = makeAttribute(styles);
					try {
						cc.sd_display.insertString(cc.sd_display.getLength(), "["+nickName+"]"
																				+" : "+message+"\n", sas);
					}catch (Exception e) {
						System.out.println(e.toString());
						}
					}
					cc.jtp_display.setCaretPosition(cc.sd_display.getLength());					
				}break;				
```

* 20-24 : String배열에 스타일들을 담아 메서드에 한번에 보내 처리한다.
* 25-17번 : 화면에 메세지를 띄우되, sas로 작성된 스타일 대로 insert한다.

```java
public SimpleAttributeSet makeAttribute(String style[]) {
		SimpleAttributeSet sas = new SimpleAttributeSet();
		//폰트컬러
		sas.addAttribute(StyleConstants.CharacterConstants.Foreground, new Color(Integer.parseInt(style[0])));
		//폰트타입
		switch(style[1]) {
		case "Font.ITALIC":
			sas.addAttribute(StyleConstants.CharacterConstants.Italic, true);
		break;
		case "Font.BOLD":
			sas.addAttribute(StyleConstants.CharacterConstants.Bold, true);
		break;
		}
		//폰트사이즈
		sas.addAttribute(StyleConstants.CharacterConstants.FontSize, Integer.parseInt(style[2]));
		
		return sas;
	}
```

* 파라미터로 받은 속성, 스타일을 적용시키기위한 메서드
* 2번 : 속성을 작성해주는 SimpleAttributeSet클래스 생성
* 4번 :  위의 sas에 파라미터로 받아온 색상을 지정, 작성해준다.
* 7-12번 : sas에 파라미터로 받은 style값에 따라 폰트를 작성한다.
* 15번 : sas에 파라미터로 받은 폰트 사이즈를 작성한다. 

