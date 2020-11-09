# BandClientThread

역할 : ServerThread에서 말하는 메세지를 받아서 화면에 띄워주는 멀티스레드 관리 클래스

## BandClientThread.java

### 선언부, 생성자

```java
package net.tomato_step2;

import java.util.StringTokenizer;
import java.util.Vector;

public class BandClientThread extends Thread {
	
	BandClient bc = null;

	public BandClientThread(BandClient bc) {
		this.bc = bc;
	}
```

### run 메서드

ServerThread에서 말해준 msg를 Client화면에 내보내기

```java
	public void run() {
		boolean isStop = false;
		while(!isStop) {
			try {
				String msg = "";
				msg = (String)bc.ois.readObject();
				StringTokenizer st = null;//해당사항이 없을 수 있어 생성은 따로한다.
				int protocol = 0;
```

#### 메세지가 비어있으면

```java
				if(msg!=null) {
					st = new StringTokenizer(msg,"#");
					protocol = Integer.parseInt(st.nextToken());//번호를 선언해둔 변수에 담는다.
				}///end of if
```

#### 100번 이라면

```java
				case 100:{
					String nickName = st.nextToken();
					bc.jta_display.append(nickName+"님이 입장하셨습니다."+"\n");
					Vector<String> v = new Vector<>();
					v.add(nickName);
					bc.dtm.addRow(v);
				}break;
```

#### 200번 이라면

```java
				case 200:{
					String nickName = st.nextToken();
					String chat = st.nextToken();
					bc.jta_display.append("["+nickName+"]"+" : "+chat+"\n");
				}break;
```

### 500번이라면, 마감

```java
				case 500:{
					String nickName = st.nextToken();
					bc.jta_display.append("["+nickName+"]"+"님이 퇴장하셨습니다."+"\n");					
					for(int i=0;i<bc.dtm.getRowCount();i++) {//새로고침에서 사용했었던 함수
						String n = (String)bc.dtm.getValueAt(i, 0);
						if(n.equals(nickName)) {//로우와 나간 닉네임이 같다면
							bc.dtm.removeRow(i);
						}
					}//end of for
				}break;
				}//end of switch
			} catch (Exception e) {
				// TODO: handle exception
			}
		}//end of while		
	}/////////////////////////////end fo run///////////////////////////////////
}
```

