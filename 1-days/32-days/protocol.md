# Protocol

### Protocol.java

```java
package net.tomato_step3;
//기존에 메세지를 구분하는 지표였던 번호를 더 직관적인 이름을 부여해주는 클래스
public class Protocol {
	//입장
	public static final int LOGIN  = 100;
	//다자간 대화
	public static final int MULTI    = 200;
	//1:1 대화
	public static final int WHISPER  = 210;
	//대화명변경
	public static final int CHANGE   = 300;
	//이모티콘 사용
	public static final int EMOTICON = 400;
	//퇴장
	public static final int CLOSE    = 500;
}
```

* 기존에 구분번호로 사용하던 것을 주관적인 이름을 붙여 한 클래스에서 관리한다.

