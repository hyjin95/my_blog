# TomatoClientThread - img넣기

### 선언부

```java
package net.tomato_step3;

public class TomatoClient extends JFrame implements ActionListener{
	Image back = getToolkit().getImage("C:\\workspace_java\\dev_java\\src\\net\\tomato_step3\\ogu_background.jpg");//현재경로
			
	Socket socket = null;//소켓2
	ObjectInputStream  ois = null;//듣기
	ObjectOutputStream oos = null;//말하기
	
	JTextArea   jta_display = new JTextArea() {
		
		public void paint(Graphics g) {
			g.drawImage(back, 0, 0, this);
			Point p = jsp_display.getViewport().getViewPosition();
			g.drawImage(back, p.x, p.y, this);
			paintComponent(g);
		}
	};
```

### 화면구현

```java
public void initDisplay() {
		//사용자로부터 닉네입 입력받기		
		//nickName = JOptionPane.showInputDialog("닉네임을 입력하세요.");
		
		jta_display.setOpaque(false);
		jta_display.setEditable(false);
		jta_display.setLineWrap(true);//자동줄바꿈처리
```

