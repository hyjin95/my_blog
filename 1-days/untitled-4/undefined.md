# 단톡방 관리 구현

## Room

{% page-ref page="../untitled-3/undefined/room.md" %}

## RoomSimulation

### main 선언부

```java
package net.tomato_step4;

import java.util.List;
import java.util.Vector;

public class RoomSimulation {

	public static void main(String[] args) {
		List<Room> roomList = new Vector<>();
		Room room = new Room();
```

### main 실행문

```java
		room.setTitle("자바 69기");
		room.setCurrent(10);
		roomList.add(room);
		List<String> nameList69 = new Vector<>();
		nameList69.add("이순신");
		nameList69.add("한석봉");
		nameList69.add("유관순");
		room.setNameList(nameList69);

		room = new Room("자바 54기",5);
		roomList.add(room);
		List<String> nameList54 = new Vector<>();
		nameList54.add("니코");
		nameList54.add("릴리아");
		nameList54.add("아리");
		room.setNameList(nameList54);
		
		room = new Room("자바 70기",13);
		roomList.add(room);		
		List<String> nameList70 = new Vector<>();
		nameList70.add("오상기");
		nameList70.add("한영진");
		nameList70.add("신민경");
		room.setNameList(nameList70);
		
		List<ChatServerThread> userList = new Vector<>();
```

### main 출력문

```java
		System.out.println("방 갯수 = "+roomList.size());//3출력
		
		for(int i=0;i<3;i++) {
			Room r = roomList.get(i);
			System.out.print(r.getCurrent()+" ");
			System.out.println(r.getTitle());
			
			for(int inwon=0;inwon<r.nameList.size();inwon++) {
				System.out.println(r.nameList.get(inwon)+" ");
			}
			System.out.println();
		}
	}
}
```

## ChatServer

### 선언부

```java
public class ChatServer extends JFrame implements Runnable, ActionListener{
    List<Room> roomList = null;
```

### run 메서드

```java
@Override
	public void run() {//서버소켓과 클라이언트 소켓을 연결한다.
		roomList = new Vector<Room>();
```



