# Login

## 참조

#### 기존에 만들어둔 화면클래스를 복사해와 사용한다.

{% page-ref page="../21-days/undefined/" %}



## Login.java

### 선언부

```java
package net.tomato_step2;

import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

public class Login extends JFrame implements ActionListener {
	
	JTextField jtf_id = new JTextField("test");
	JTextField jpf_pw = new JTextField("123");
	
	Font f = new Font("나눔고딕",Font.BOLD,17);
	
	BandClient bc = null;
	BendMemberShip bmb = null;	
	
	String nickName = null;
```

### initDisplay - 화면처리 메서드

```java
//화면처리부
public void initDisplay() {//setBound로 id창+tf, pw창+tf2개
		
	jbtn_login.addActionListener(this);
	jbtn_join.addActionListener(this);	
}
```

### main 메서드

```java
//메인메서드
public static void main(String[] args) {
	Login lf = new Login();
	lf.initDisplay();
}
```

## actionPerformed-login버튼

```java
	@Override
	public void actionPerformed(ActionEvent e) {
		Object obj = e.getSource();//이벤트가 감지된 주소번지 달기
		
		if(obj==jbtn_login) {
```

### 미입력시

```java
	if("".equals(jtf_id.getText())) {
				JOptionPane.showMessageDialog(this, "아이디를 확인하세요."
											   , "INFO"
											   , JOptionPane.INFORMATION_MESSAGE);
				return;//탈출
			}
			else if("".equals(jpf_pw.getText())) {
				JOptionPane.showMessageDialog(this, "비밀번호를 확인하세요."
											   , "INFO"
											   , JOptionPane.INFORMATION_MESSAGE);
				return;//탈출
			}
```

### 입력시 - DB 조회, 비교

```java
		else {
			String mem_id = jtf_id.getText();
			String mem_pw = jpf_pw.getText();
			BandDao bdao = new BandDao();
			nickName = bdao.procLogin(mem_id, mem_pw);
			if("아이디가 존재하지 않습니다.".equals(nickName)) {
				JOptionPane.showMessageDialog(this, "아이디를 확인하세요."
											   , "INFO"
												  , JOptionPane.INFORMATION_MESSAGE);					
			}
			else if("비밀번호가 틀립니다.".equals(nickName)) {
				JOptionPane.showMessageDialog(this, "비밀번호를 확인하세요."
												  , "WARN"
												  , JOptionPane.WARNING_MESSAGE);					
			}else {
				this.dispose();//this의 원래 화면을 닫는다.
				bc = new BandClient(nickName);
				bc.initDisplay();
				bc.init();
			}
		 }
	 }//end of if
	}//end of actionPerformed
}//end of class
```

