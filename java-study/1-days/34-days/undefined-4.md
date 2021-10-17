# 이모티콘 구현

## PictureMessage.java

이모티콘으로 사용할 이미지, gif 관리 클래스, 사용자가 화면의 이모티콘 버튼 클릭시 실행되는 화면

### 선언부

```java
package net.tomato_step4;

import java.awt.Color;
import java.awt.GridLayout;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

import javax.swing.ImageIcon;
import javax.swing.JButton;
import javax.swing.JDialog;
import javax.swing.JPanel;

public class PictureMessage extends JDialog implements ActionListener{
	JPanel jp = new JPanel();
	GridLayout gl = new GridLayout(2,4,3,3);
	ImageIcon imgs[] = new ImageIcon[8];
	JButton jbtn_0 = new JButton();
	JButton jbtn_1 = new JButton();
	JButton jbtn_2 = new JButton();
	JButton jbtn_3 = new JButton();
	JButton jbtn_4 = new JButton();
	JButton jbtn_5 = new JButton();
	JButton jbtn_6 = new JButton();
	JButton jbtn_7 = new JButton();
	
	String  imgNames[] = {"ogu1.png", "ogu2.png", "ogu3.png", "ogu4.png"
																	, "ogu5.png", "ogu6.png", "ogu7.png", "ogu8.png"};
	JButton jbtns[]    = {jbtn_0, jbtn_1, jbtn_2, jbtn_3, jbtn_4, jbtn_5
																	                    , jbtn_6, jbtn_7};
	
	String imgChoice = "default";
	String path = "C:\\Users\\user\\Desktop\\web\\work_space\\dev_java\\src\\net\\tomato_step4\\";
```

* client화면에 종속시키기위해 JDialog클래스를 extends
* ImageIcon클래스의 배열을 선언, 생성
* 이모티콘으로 사용할 이미지의 이름을 담을 배열을 선언, 생성
* 버튼을 담는 배열을 선언, 생성
* 대화 도중 이미지선택시 이모티콘을 출력시키기위해 default값을 넣어준다.\
  \- 대화처리와 같은 부분에서 처리해야 할 것이다.
* 이미지 주소를 담는 String타입 변수를 선언, 생성

### 생성자 & main

```java
	public PictureMessage() {
		initDisplay();
	}	
	public static void main(String[] args) {
		PictureMessage pm = new PictureMessage();
	}///////////////////////////end of main//////////////////////////////

```

* main메서드를 통해 생성자로 화면을 호출

### 화면

```java
	public void initDisplay() {
		jp.setLayout(gl);
		for(int i=0;i<jbtns.length;i++) {
			imgs[i] = new ImageIcon(path+imgNames[i]);
			jbtns[i].setIcon(imgs[i]);
			jp.add(jbtns[i]);
		}
		
		jbtn_0.setBackground(new Color(218,217,255));
		jbtn_1.setBackground(new Color(218,217,255));
		jbtn_2.setBackground(new Color(218,217,255));
		jbtn_3.setBackground(new Color(218,217,255));
		jbtn_4.setBackground(new Color(218,217,255));
		jbtn_5.setBackground(new Color(218,217,255));
		jbtn_6.setBackground(new Color(218,217,255));
		jbtn_7.setBackground(new Color(218,217,255));
		
		jbtn_0.addActionListener(this);
		jbtn_1.addActionListener(this);
		jbtn_2.addActionListener(this);
		jbtn_3.addActionListener(this);
		jbtn_4.addActionListener(this);
		jbtn_5.addActionListener(this);
		jbtn_6.addActionListener(this);
		jbtn_7.addActionListener(this);
		
		this.add("Center",jp);
		this.setSize(460, 260);
		this.setVisible(false);
	}/////////////////////////end of initDisplay//////////////////////////\
```

* 2번 : JPanel의 Layout을 gridLayout으로 바꾼다.
* 3번 : for문과 생성된 버튼 배열을 이용해 JPanel에 버튼을 add한다.
* 4번 : 선언부에서 생성한 ImageIcon타입 배열을 새로 생성한다.\
  \- 파라미터로 이미지주소와 이름배열을 갖는다.\
  \- ImageIcon타입 배열이 이미지 주소를 담게된다.
* 5번 : 선언부에서 생성한 버튼 배열에 위에서 만든 이미지 주소배열을 setIcon함수를 이용해 set시킨다.
* 6번 : 이렇게 만든 이미지가 붙은 버튼을 JPanel에 add한다.
* 29번 : 이 클래스 호출전에는 보여지지 말아야하는 화면이므로 false로 감춰둔다.

### actionPerformed

```java
	@Override
	public void actionPerformed(ActionEvent ae) {
		//JVM이 이벤트를 감지하면 호출, 콜백메서드
		Object obj = ae.getSource();
		if(obj==jbtn_0) {
			imgChoice = "ogu1.png";
			//System.out.println("선택한 이모티콘 -> "+imgChoice);
		}
		else if(obj==jbtn_1) {
			imgChoice = "ogu2.png";
			//System.out.println("선택한 이모티콘 -> "+imgChoice);
		}
		else if(obj==jbtn_2) {
			imgChoice = "ogu3.png";
			//System.out.println("선택한 이모티콘 -> "+imgChoice);
		}
		else if(obj==jbtn_3) {
			imgChoice = "ogu4.png";
			///System.out.println("선택한 이모티콘 -> "+imgChoice);
		}
		else if(obj==jbtn_4) {
			System.out.println("선택한 이모티콘 -> "+imgChoice);
			//imgChoice = "ogu5.png";
		}
		else if(obj==jbtn_6) {
			imgChoice = "ogu6.png";
			//System.out.println("선택한 이모티콘 -> "+imgChoice);
		}
		else if(obj==jbtn_7) {
			imgChoice = "ogu7.png";
			//System.out.println("선택한 이모티콘 -> "+imgChoice);			
		}
	}
}
```

* 화면의 버튼이 눌리면 멤버변수로 선언한 imgChoice 변수를 해당 이미지이름으로 초기화한다.
* server나 serverthread, clientthread에서 구분하기 위함

## ChatClient

### 선언부

```java
public class ChatClient extends JFrame implements ActionListener{
    PictureMessage pm = new PictureMessage();
```

* 만든 이모티콘구현 클래스를 인스턴스화한다.

### actionPerformed-1

```java
@Override
	public void actionPerformed(ActionEvent e) {
		Object obj = e.getSource();
		String msg = jtf_msg.getText();
		if(obj==jbtn_send || obj == jtf_msg) {
			//깔대기 --2
			if(msg==null || msg.length()==0) {//200#닉네임## <- 방지
				msg = "이모티콘";
			}
			try {
				oos.writeObject(Protocol.MULTI+"#"+nickName
							 				  +"#"+msg
							 				  +"#"+fontColor
							 			    +"#"+fontStyle
							 				  +"#"+fontSize
							 				  +"#"+pm.imgChoice);
				jtf_msg.setText("");
			} catch (Exception e2) {
				System.out.println(e2.toString());
			}	
		}////////////////////////jbtn_send, jtf_msg//////////////////////////
```

* 기존의 기본 대화 전송에 같이 구현한다.
* jtf_msg에서 가져온 text의 기본값은 null이 아닌 " ", 빈 문자열이 들어있다.
* 7,8번 : 대화입력없이 이모티콘만 전송하는 경우, 200#닉네임##선택한이모티콘 이렇게 될 수 있으므로   \
               \##을 방지하기위해 넣은 if문
* 10번 : server와 연동하므로 예외처리가 필수
* 11-16번 : 기존의 말하기 내용에 이모티콘 자리를 마련해준다.
* 17번 : 입력창 초기화

### actionPerformed-2

```java
else if(obj==jbtn_emo) {//--3
			pm.setVisible(true);			
		}
```

* 이모티콘 버튼을 누르면 구현해둔 이모티콘선택 화면을 띄운다.

## ChatServerThread

### run메서드

```java
public void run() {//ChatServer에서 thread가 배분된 다음 호출
		String msg = null;
		boolean isStop = false;
		try {
			run_start: //while문 탈출라벨
			while(!isStop) {
				msg = (String)ois.readObject();//듣기 시작
				cs.jta_log.append(msg+"\n");
				cs.jta_log.setCaretPosition(cs.jta_log.getDocument().getLength());
				StringTokenizer st = null;//선언과 생성을 왜 분리할까?
				int protocol = 0;
				if(msg!=null) {//메세지가 있다면
					st = new StringTokenizer(msg,"#");//메세지가 없는경우에는 실행할 수 없다.
					protocol = Integer.parseInt(st.nextToken());					
				}//end of if
				switch(protocol) {//protocol은 if문에서 token으로 꺼낸다
				//default://case 없이 단위테스트하기
				//System.out.println("protocol"+protocol);
				case Protocol.MULTI:{//다자간대화
					String nickName  = st.nextToken();
					String message   = st.nextToken();
					String fontColor = st.nextToken();
					String fontStyle = st.nextToken();
					String fontSize  = st.nextToken();
					String imgChoice = "";
					if(st.hasMoreTokens()) {//--깔대기 4
						imgChoice = st.nextToken();//token이 남아 있다면 이모티콘이다.
					}
					broadCasting(Protocol.MULTI+"#"+nickName+"#"+message+"#"+fontColor
											   +"#"+fontStyle+"#"+fontSize+"#"+imgChoice);//token이 없다면 ""로 둔다.
				}break;
					
```

* 10번에서 StringTokenizer를 선언하는 이유는 if문에서도 switch문에서도 사용해야하기 때문이다.\
  \- switch문을 이용하려면 구분번호인 protocol을 먼저 추출해야 한다. \
  \- if문+Interger.parseInt( );
* 16-18 : switch문 단위테스트
* 25번 : 우리는 이모티콘 멤버변수에 default값을 주었지만 주지않은 경우에는 nextToken이 공백이라 진행되지 않으므로 " "으로 초기화시킨다.
* 26-28번 : 이모티콘을 받지않았으므로 token이 남아있다면 이모티콘 값이다. 그때 담는다.
* 29번 : 말하기, 이모티콘 멤버변수에 기본값을 주거나, 주지않더라도 25-28번을 진행했으면 괜찮지만 그렇지 않은 경우에 이모티콘이 없다면 nullPointerException이 발생할 수 있다.

## ChatClientThread

### 선언부

```java
public class ChatClientThread extends Thread {
	
	String path = "C:\\Users\\user\\Desktop\\web\\work_space\\dev_java\\src\\net\\tomato_step4\\";
```

* 이모티콘 주소번지를 멤버변수에 선언, 생성한다.

### run 메서드 -1

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
					String imgChoice = st.nextToken();
					//JOptionPane.showMessageDialog(cc,"imgChoice : "+imgChoice);//단위테스트
					MutableAttributeSet sas2 = new SimpleAttributeSet();//이모티콘을 위한 스타일
```

* 기존의 대화 처리 구문에서 구현한다.
* 24번 : 이모티콘을 받는다. 없으면 " ", 있으면 이모티콘의 이름이 담긴다.
* MutableAttributeSet 클래스타입의 변수를 simpleAttributeSet메서드로 구현한다.\
  \- 이모티콘을 위한 스타일 생성

### 이모티콘을 선택한 경우

```java
		if(!imgChoice.equals("default")) {//default가 아닌 이모티콘을 담고있다면
			int i = 0;
			for(i=0;i<cc.pm.imgNames.length;i++) {
				if(cc.pm.imgNames[i].equals(imgChoice)) {
					StyleConstants.setIcon(sas2, new ImageIcon(path+cc.pm.imgNames[i]));
					try {
						cc.sd_display.insertString(cc.sd_display.getLength(), "\n", sas2);
						cc.pm.imgChoice="default";
					} catch (Exception e) {
						System.out.println(e.toString());
					}
				}
			}
		}////////////////////////end of 이모티콘 일때/////////////////////////////////					
```

* 1번 : 이모티콘을 구분하는 멤버변수의 값이 설정해둔 기본값이 아니라, 이모티콘이름 이라면
* 3번 : 이모티콘이름을 담는 배열안의 이름 갯수만큼 반복한다.
* 4,5번 : 배열안의 이름과 사용자가 선택한 이모티콘값이 같다면 sas2변수의 스타일을 Icon으로 set한다. Icon = new ImaegeIcon(path+cc.pm.imgNames\[i])\
  \- 스타일에 해당 이모티콘을 담는다.
* 7번 : 사용자 화면에 이모티콘이 담긴 스타일을 출력한다.
* 8번 : 다음 이모티콘 선택값을 위해 다시 기본값으로 초기화 한다.

### 일반 대화인 경우

```java
		///////////////////////////일반 메세지 일때////////////////////////////////////
		else if(imgChoice.equals("default")) {
			SimpleAttributeSet sas = makeAttribute(styles);
			try {
				cc.sd_display.insertString(cc.sd_display.getLength(), "["+nickName+"]"+" : "+message+"\n", sas);
			} catch (Exception e) {
				System.out.println(e.toString());
			}
		}
		cc.jtp_display.setCaretPosition(cc.sd_display.getLength());					
	}break;
```

* 2번 : 이모티콘을 구분하는 멤버변수의 값이 설정해둔 기본값이라면 일반대화인 경우이다.
* 3번 : 이모티콘이 아닌 문자를 출력하므로 문자에 대한 style을 생성한다. 
* 5번 : 사용자 화면에 대화 출력
