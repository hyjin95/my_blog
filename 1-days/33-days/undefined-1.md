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

