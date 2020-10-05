# 대기실 구현\(상\)

## Login

### 선언부

```java
public class Login extends JFrame implements ActionListener {
    ChatClientVer2 ccv2 = null;
    String nickName = null;
```

### actionPerformed - 로그인성공

```java
@Override
	public void actionPerformed(ActionEvent e) {
		Object obj = e.getSource();//이벤트가 감지된 주소번지 달기
		}else {
					jtf_id.setText("");
					jpf_pw.setText("");
					this.dispose();//this의 원래 화면을 닫는다.
					JOptionPane.showMessageDialog(this, nickName + "님의 접속을 환영합니다."
													  , "INFO"
													  , JOptionPane.INFORMATION_MESSAGE);					
					ccv2 = new ChatClientVer2(this);
				}
```

## ChatClientVer2

## WaitRoom

## Room

## 

