# Room

```java
package net.tomato_step4;

import java.util.List;
import java.util.Vector;

//현재 단톡방에 들어와있는 사용자 스레드만 관리할 수 있는 클래스

public class Room {	
	List<ChatServerThread> userList = new Vector<>();
	
	//사용자의 닉네임을 따로 관리 하려면
	List<String> nameList = new Vector<>();
	String title = null;//톡방명 관리 변수
	String state = null;//현재상태 관리 변수(대기실, 참여중, ...)
	//현재 정원
	int current = 0;
	//최대 정원수
	int max = 0;
	
	public Room() {}
	public Room(String title, String state, int current) {
		this.title = title;
		this.state = state;
		this.current = current;
	}
	
	public List<ChatServerThread> getUserList() {
		return userList;
	}
	public void setUserList(List<ChatServerThread> userList) {
		this.userList = userList;
	}
	public List<String> getNameList() {
		return nameList;
	}
	public void setNameList(List<String> nameList) {
		this.nameList = nameList;
	}
	public String getTitle() {
		return title;
	}
	public void setTitle(String title) {
		this.title = title;
	}
	public String getState() {
		return state;
	}
	public void setState(String state) {
		this.state = state;
	}
	public int getCurrent() {
		return current;
	}
	public void setCurrent(int current) {
		this.current = current;
	}
	public int getMax() {
		return max;
	}
	public void setMax(int max) {
		this.max = max;
	}
}
```

