---
description: 2020.10.06 - 36일차
---

# 36 Days - 대기실&톡방 생성, 관리 구현, 방 입장 구현, 네이버 api

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261 : Oracle.com
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5

## 복습

### level 1 : Talk

* 서버구동 - client에서 닉네임 입력 - server에서 접속 성공시 thread부여\(생성자에 this 객체주입을 통한 인스턴스화\), 소켓동기화 - serverThread에서 멀티스레드 관리 - clientThread에서 server의 말 화면에 출력
* 닉네임 입력과 동시에 서버에 100\#닉네임을 전송

### level 2 : Band

* 로그인 구현
* DB연동  - 로그인을 위한 프로시저 호출.
* Dao 추가 - 정보의 영속성을 위해 oracle을 연동하는 클래스 - Server클래스에서 연동하지 않는 이유   종료시 데이터가 유지되지 않기 때문이다.
* Dao에서 유지하는 데이터 : 회원가입, 로그인정보
* server에서 유지하는 데이터 : 접속자 정보\(닉네임\)

### level 3 : Tomato

* Protocol추가 - 기존에 사용하던 구분 번호대신 직관적인 변수를 도입했다.
* 1대1 대화구현
* 대화명 변경 구현
* 스타일 추가

### level 4 : Chat

* Login에 성공하면 chatClientVer2의 생성자를 객체주입을 통해 호출한다.\(인스턴스화\)
* chatClientVer2의 생성자 - 화면처리, 소켓생성, 소켓연결, 로그인 성공\(대기상태\)알림 말하기, clientThread객체주입 인스턴스화, run메서드 실행을 통한 듣기 준비
* 이모티콘 구현
* 대기실 구현
* 단톡방 구현

### 테스트 시나리오

* 단위 테스트가가능 -&gt; 통합테스트 가능 -&gt; 운영서버에 배포, 이행이 가능

## 대기실&톡방 생성, 관리 구현

### Room

* 방을 구분하기위한 변수가 필요하다. 방마다 다른 값을 같는 변수를 멤버변수에 선언한다. - title, current - 사용자의 thread에 따라 달라질 수 있다.
* List 1 :  List&lt;ChatServerThread&gt; userList  - 톡방에 있는 접속자들의 thread정보가 담긴 List타입의 Vector - thread를 담고 있어 send의 oos를 사용할 수 있다. - 해당 톡방의 접속자들에게 말하려면 userList를 사용해야 할 것이다.
* List 2 : List&lt;String&gt; nameList -  톡방에 있는 접속자들의 닉네임이 담긴 List타입의 Vector - String값일 뿐이므로 send의 oos를 사용할 수 없다.

### ChatServer

* List : List&lt;Room&gt; roomList - 모든 톡방들의 정보를 담아 유지하는 List - 값이 담기는 곳   톡방이 생성되는곳, ChatServerThread클래스의 run메서드 안에 Protocol.CREATE case안에서

### WaitRoom

* 대기실 화면 구현 클래스, actionPerformed를 통해 이벤트를 구현한다. - actionPerformed -&gt; oos사용 -&gt; 소켓이 필요하다. - clientVer2에 구현된 oos를 사용한다.
* 방이 선택되면 JTable에서 해당 선택값을 가져와야한다. - 소유주.getRowCount\( \);함수를 사용해 대기 방 Table의 row 수 만큼 for문을 돌린다. - 소유주.isRowSelected\(index\)함수를 사용해 선택된 로우에서 if문을 통해 값을 담는다. - 소유주.getValueAt\(index, colum\)함수를 이용해 해당 row의 colum정보를 변수에 담는다. - 필요한 정보 : 선택된 톡방, 입장할 접속자 thread의 nickName

### ChatServerThread

* 로그인에 성공하면 바로 채팅방에 접속되는 것이 아니라 대기실에 있게 된다. - clientVer2클래스에서 대기실에 입장했음을 말하는 것을 듣고 clientThread에 넘긴다.
* 해당 톡방에 접속해있는 사람들에게만 전송하는 메서드를 구현해보자.

### ChatClientThreadVer2

* ServerThread에서 대기실 입장을 알려주면 WaitRoom 클래스의 화면에 출력한다. - JTable에 대기실에 입장한 thread의 nickName과 위치\(대기 \|\| 대화방\)를 출력하자. - Vector에 담아 dtm에 addRow한다.
* ServerThread에서 방 생성을 알려주면 WaitRoom 클래스의 화면에 출력한다. - JTable에 생성된 방과 방의 현재입장인원을 출력하자. - Vector에 담아 dtm에 addRow한다.

### 테이블 값 증가에 따른 스크롤바 이동

* AdjustmentListener 인터페이스를 사용해 구현한다. - 스크롤바의 현재위치를 읽고 스크롤바가 이동될때 AdjustmentEvent가 발생한다.
* 테이블의 세로 스크롤바만 이동할 여지가 있으므로 소유주.getVerticalScrollBar함수를 사용해서 소유주의 스크롤바에 addAdjustmentListener하여 adjustmentValueChanged메서드를 오버라이드 한다.
* JScrollBar타입의 변수에 \(JScrollBar\)e.getSource\( \);하여 소스를 받아온다.
* 변수.setValue\(변수.getMaximum\(\)\); 를 이용해 스크롤바의 최대위치에 set한다.

{% page-ref page="undefined.md" %}

## 대화방 입장 이벤트 구현

### WaitRoom

* 
### ChatServerThread

* 
### ChatclientThread

* 
{% page-ref page="undefined-2.md" %}

후기 : 

