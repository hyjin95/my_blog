# Protocol

```java
package net.tomato_step4;
//기존에 메세지를 구분하는 지표였던 번호를 더 직관적인 이름을 부여해주는 클래스
public class Protocol {
	//대기상태
	public static final int WAIT  		 = 100;
	//단톡방 개설
	public static final int ROOM_CREATAE = 200;
	//방목록 처리
	public static final int ROOM_LIST 	 = 210;
	//방 입장
	public static final int ROOM_IN	 	 = 220;
	//방 입장 인원 목록
	public static final int ROOM_INLIST  = 230;
	//방 퇴장
	public static final int ROOM_OUT	 = 290;
	//입장
	//public static final int LOGIN 	 = 300;
	//다자간 대화
	public static final int MULTI    	 = 300;
	//1:1 대화
	public static final int WHISPER  	 = 310;
	//대화명변경
	public static final int CHANGE  	 = 320;
	//퇴장
	//public static final int CLOSE    	 = 500;
	//대화내용 사이의 토큰
	public static final String seperator = "|";
}

```

