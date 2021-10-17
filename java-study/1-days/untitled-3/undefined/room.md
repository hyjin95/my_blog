# Room - getter, setter

### 선언부

```java
package net.tomato_step4;

import java.util.List;
import java.util.Vector;

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
```

* 현재 단톡방에 접속해 있는 사용자의 스레드만 관리하는 클래스\
  \- 현재 단톡방 접속자의 닉네임, 현재 정원, 최대 정원, 단톡방 이름,...을 관리하는 클래스
* 여러 thread를 관리해야한다.\
  \- List\<ChatServerThread> 를 Vector로 선언, 생성
* 접속자들의 모든 닉네임을 관리해야한다.\
  \- List\<String>을 Vector로 선언, 생성

### 생성자

```java
	public Room() {}
	public Room(String title, String state, int current) {
		this.title = title;
		this.state = state;
		this.current = current;
	}
```

* 톡방명, 상태, 현재 정원 수 를 파라미터로 가져와 단톡방을 구분하는 생성자를 구현
* 멤버변수를 파라미터 값으로 초기화한다.\
  \- 단톡방마다 다른 값을 가질 것이므로

### getter\&setter

```java
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
