---
description: 2020.10.06 - 36일차
---

# 36 Days - 대기실&톡방 생성, 관리 구현, 방 입장 구현, 네이버 api, Vector, ArrayList차이

### 사용 프로그램

* 사용언어 : JAVA(JDK)1.8.0\_261 : Oracle.com
* 사용Tool \
  \- Eclipse : Eclipse.org\
  \- Toad DBA Suite for Oracle 11.5

## 복습

### level 1 : Talk

* 서버구동\
  \- client에서 닉네임 입력\
  \- server에서 접속 성공시 thread부여(생성자에 this 객체주입을 통한 인스턴스화), 소켓동기화\
  \- serverThread에서 멀티스레드 관리\
  \- clientThread에서 server의 말 화면에 출력
* 닉네임 입력과 동시에 서버에 100#닉네임을 전송

### level 2 : Band

* 로그인 구현
* DB연동 \
  \- 로그인을 위한 프로시저 호출.
* Dao 추가\
  \- 정보의 영속성을 위해 oracle을 연동하는 클래스\
  \- Server클래스에서 연동하지 않는 이유\
    종료시 데이터가 유지되지 않기 때문이다.
* Dao에서 유지하는 데이터 : 회원가입, 로그인정보
* server에서 유지하는 데이터 : 접속자 정보(닉네임)

### level 3 : Tomato

* Protocol추가\
  \- 기존에 사용하던 구분 번호대신 직관적인 변수를 도입했다.
* 1대1 대화구현
* 대화명 변경 구현
* 스타일 추가

### level 4 : Chat

* Login에 성공하면 chatClientVer2의 생성자를 객체주입을 통해 호출한다.(인스턴스화)
* chatClientVer2의 생성자\
  \- 화면처리, 소켓생성, 소켓연결, 로그인 성공(대기상태)알림 말하기, clientThread객체주입 인스턴스화, run메서드 실행을 통한 듣기 준비
* 이모티콘 구현
* 대기실 구현
* 단톡방 구현

### 테스트 시나리오

* 단위 테스트가가능 -> 통합테스트 가능 -> 운영서버에 배포, 이행이 가능

### Synchronize(동기화)

* 스레드가 하나의 자원을 놓고 락을 가진 스레드부터 차례대로 그 자원을 사용하고 다음 스레드에게 락과 함께 자원을 반납하는 것.
* 동기화가 이루어지면 한번에 한 스레드만 수행할수 있으므로 주의해야한다.

### Vecotr와 ArrayList의 차이

* Vector\
  \- 크기가 자유로운 동적 배열 구현\
  \- 정수 인덱스를 이용해 배열에 접근할 수 있다.\
  \- Synchronize, **동기화가 되어있어 한번에 하나의 스레드만 접근**이 가능하다.\
  \- 스레드가 순서대로 수행함으로 멀티스레드에서 안전하다.\
  \- 배열이 최대 인덱스를 초과하면 현재 배열 크기에 100%가 증가한다.
* ArrayList\
  \- 크기가 자유로운 동적 배열 구현\
  \- 기본데이터타입(int, char,..)로는 만들수 없어 Integer, Object등의 객체에 대해 참조해야한다.\
  \- **동기화되지 않아 동시에 여러 스레드가 접근**이 가능하다. 벡터보다 빠르다.\
  \- 싱글스레드에서 안전하다.\
  \- 배열이 최대 인덱스를 초과하면 현재 배열 크기에 50%가 증가한다.
* 멀티스레드 환경인 경우에는 Vector를, 멀티스레드 환경이 아니라면 ArrayList를 사용한다.

### 인터페이스를 사용하는 이유

* 왜 이벤트 처리시에는 인터페이스를 구현하는 것으로 설계를 할까?\
  \- impelements ActionListener
* 버튼은 동일하더라도 그 기능으 각각의 디바이스(장치)에 따라 다를 수 있다.\
  \- 장치마다 다르게 동작하도록 조작할 수 있어야한다.
* 상속을 하는 경우에는 필요없는 메서드까지도 오버라이딩을 강요당할 수 있다.
* **인터페이스로 사용하면 상속관계가 아니므로 강제조건 없이 필요한 메서드만 오버라이딩**할 수 있다.
* 비교적 자유롭게 구현이 가능하고, 특정 클래스에 종속적이지 않게 되므로 **재사용성이 높다.**
* 결합도, 의존성이 낮으므로 개발 기간이 단축되고, 테스트도 더 빠르게 해결될 수 있다.
* 클래스 설계 시에 많이 사용되며, 개발 4년차 이상 되었을때 설계를 담당할 수 있다.\
  \- 업무를 알아야한다.
* 게임엔진 설계나 프레임워크 개발에도 추상클래스, 인터페이스 중심의 설계, 개발을 해야한다.

## 대기실&톡방 생성, 관리 구현

### Room

* 방을 구분하기위한 변수가 필요하다. 방마다 다른 값을 같는 변수를 멤버변수에 선언한다.\
  \- title, current\
  \- 사용자의 thread에 따라 달라질 수 있다.
* List 1 :  List\<ChatServerThread> userList \
  \- 톡방에 있는 접속자들의 thread정보가 담긴 List타입의 Vector\
  \- thread를 담고 있어 send의 oos를 사용할 수 있다.\
  \- 해당 톡방의 접속자들에게 말하려면 userList를 사용해야 할 것이다.
* List 2 : List\<String> nameList\
  \-  톡방에 있는 접속자들의 닉네임이 담긴 List타입의 Vector\
  \- String값일 뿐이므로 send의 oos를 사용할 수 없다.

### ChatServer

* List : List\<Room> roomList\
  \- 모든 톡방들의 정보를 담아 유지하는 List\
  \- 값이 담기는 곳\
    톡방이 생성되는곳, ChatServerThread클래스의 run메서드 안에 Protocol.CREATE case안에서

### WaitRoom

* 대기실 화면 구현 클래스, actionPerformed를 통해 이벤트를 구현한다.\
  \- actionPerformed -> oos사용 -> 소켓이 필요하다.\
  \- clientVer2에 구현된 oos를 사용한다.
* 방이 선택되면 JTable에서 해당 선택값을 가져와야한다.\
  \- 소유주.getRowCount( );함수를 사용해 대기 방 Table의 row 수 만큼 for문을 돌린다.\
  \- 소유주.isRowSelected(index)함수를 사용해 선택된 로우에서 if문을 통해 값을 담는다.\
  \- 소유주.getValueAt(index, colum)함수를 이용해 해당 row의 colum정보를 변수에 담는다.\
  \- 필요한 정보 : 선택된 톡방, 입장할 접속자 thread의 nickName

### ChatServerThread

* 로그인에 성공하면 바로 채팅방에 접속되는 것이 아니라 대기실에 있게 된다.\
  \- clientVer2클래스에서 대기실에 입장했음을 말하는 것을 듣고 clientThread에 넘긴다.
* 해당 톡방에 접속해있는 사람들에게만 전송하는 메서드를 구현해보자.

### ChatClientThreadVer2

* ServerThread에서 대기실 입장을 알려주면 WaitRoom 클래스의 화면에 출력한다.\
  \- JTable에 대기실에 입장한 thread의 nickName과 위치(대기 || 대화방)를 출력하자.\
  \- Vector에 담아 dtm에 addRow한다.
* ServerThread에서 방 생성을 알려주면 WaitRoom 클래스의 화면에 출력한다.\
  \- JTable에 생성된 방과 방의 현재입장인원을 출력하자.\
  \- Vector에 담아 dtm에 addRow한다.

### 테이블 값 증가에 따른 스크롤바 이동

* AdjustmentListener 인터페이스를 사용해 구현한다.\
  \- 스크롤바의 현재위치를 읽고 스크롤바가 이동될때 AdjustmentEvent가 발생한다.
* 테이블의 세로 스크롤바만 이동할 여지가 있으므로 소유주.getVerticalScrollBar함수를 사용해서 소유주의 스크롤바에 addAdjustmentListener하여 adjustmentValueChanged메서드를 오버라이드 한다.
* JScrollBar타입의 변수에 (JScrollBar)e.getSource( );하여 소스를 받아온다.
* 변수.setValue(변수.getMaximum()); 를 이용해 스크롤바의 최대위치에 set한다.

{% content-ref url="undefined.md" %}
[undefined.md](undefined.md)
{% endcontent-ref %}

## 대화방 입장 이벤트 구현

### WaitRoom

* actionPerformed에서 입장버튼 클릭에 대한 이벤트 처리를 한다.
* 방을 선택하지 않은 경우에 알림을 띄운다.\
  \- int 변수\[ ] = 해당jtable.getSelectedRow( );함수로 선택 값의 갯수를 담는다.\
  \- 변수.length가 0 이라면 선택값이 없는것이다.\
  \- 실행문 이후 return;으로 반드시 if문을 탈출시킨다.
* 방 목록이 JTable에 표시되므로 JTable에서 값을 꺼내야한다.\
  \- 꺼낸값을 oos를 통해 말하기 해야하므로 try문 안에서 처리한다.\
  \- 해당Jtable.getRowCount( );함수로 방 목록의 갯수만큼 for문을 돌린다.\
  \- 해당JTable.isRowSelected(i)조건으로 if문을 돌린다.\
  \- String 변수 = (String)해당JTalbe의DTM.getValueAt(i, 0); 함수로 로우 값을 꺼낸다.\
  \- 선택된 i번째 로우 중 0번째 컬럼의 로우 값\
  \- 필요한 정보가 수집됐으면 oos를 통해 서버에 전송한다.\
  \- 입장 protocol = Protocol.ROOM_IN

### ChatServerThread

* 위 waitRoom 클래스가 말한 값을 듣고 run메서드 안에서 처리한다.
* 대기실의 대화방 목록에서 입장한 대화방의 인원수를 업데이트해야한다.\
  \- Room 클래스를 인스턴스화하여 전송받은 값으로 set해야 할 것이다.
* 해당 대화방의 userList에 접속자의 thread를 추가해야한다.\
  \- Room 클래스를 인스턴스화하여 전송받은 값으로 add해야 할 것이다.
* 해당 대화방의 nameList에 접속자의 닉네임을 추가해야한다.\
  \- Room 클래스를 인스턴스화하여 전송받은 값으로 add해야 할 것이다.
* 방의 모든 접속자에게 입장 알림을 전송한다.\
  \- broadCasting
* break;로 case를 끝내고 switch문을 탈출한다.

### ChatclientThreadVer2

* 위 serverThread에서 말한 값을 듣고 run메서드 안에서 출력한다.
* 37일차에 구현

{% content-ref url="undefined-2.md" %}
[undefined-2.md](undefined-2.md)
{% endcontent-ref %}

## 네이버 API사용 준비

### 네이버 서버 이용하기

1. 네이버 로그인
2. 네이버 개발자 센터 검색
3. 서비스API

### 캡차 사용준비

1. 항목에서 '캡차'선택
2. 오픈 API신청 클릭
3. 계정 본인인증
4. 애플리케이션 등록(API이용신청)\
   \- 이름지정\
   \- 캡차(이미지)\
   \- 서비스환경 WEB
5. Eclipse에서 사용할 xml선택, port번호 확인 -> server화면 Modules탭
6. 만든어둔 index.jsp실행 후 상단의 주소를 4번의 WEB주소에 입력한다.\
   \- 주소 끝단의 도메인 이름은 지운다.

후기 : 오랜만에 운동하니 시간은 부족하지만 기분은 좋았다 ㅋㅋㅋ 오늘은 네이버의 api를 보고 사용해보기위한 사전작업을 햇는데 채팅프로그램을 수업하고잇다보니 카카오톡의 api도 찾아봐야겠다.
