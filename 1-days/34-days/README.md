# 34 Days - level4복습, 이모티콘구현

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261 : Oracle.com
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5

## level4 복습

### run메서드 호출

* 실행시킬 run메서드의 소유주를 알고 start함수를 사용한다.
* 소유주.start\( \);

### while문 무한루프

* 반복문을 사용할 때에는 항상 무한루프를 주의해야한다.
* while문의 조건안에 false,true대신 boolean타입의 변수를 이용하고, 라벨을 사용하기도한다.

### 예외처리

* 예외발생 가능성이 있는 코드에 대해서는 반드시 예외처리한다.
* java.io패키지, java.net패키지를 사용할 때
* thread를 사용할 때
* 오라클서버나 톰캣과같은 웹서버와 연동할 때
* 클라우드 서비스를 제공하는 경우

### Thread&Vector

* 동시접속이 일어나는 경우에 thread를 부여해서 관리한다. - 동시접속하는 사람들의 행동을 별개로 동작시켜야 하므로 - 스레드를 부여하면 접속자의 행동에 대한 조작이 가능해진다.
* 여러 접속자를 관리해야하므로 Vector에 접속자를 담아 관리한다. - Vector&lt;ChatServerThread&gt; - ChatServerThread타입이지만 실제로는 접속자들의 개별 thread를 담는 List이다.

### 사용자의 접속경로

1. server에서 접속자의 socket생성 : new socket
2. server에서 serverThread생성 : 생성자실행
3. '나'입장, 현재 접속자 알림을 '나'에게 전송
4. '나'포함 모두에게 입장 알림 전송
5. 사용자가 client에서 문자, 버튼 입력
6. actionPerformed실행 : 형식에 맞춰 oos.writeObject함수로 말하기
7. serverThread의 run메서드가 청취
8. run메서드가 구분protocol에 따라 broadCasting, send메서드로 말할 대상을 선정, 말하기
9. clientThread의 run메서드가 청취
10. run메서드가 구분protocol에 따라 출력, 응답 진행

### ServerThread의 oos, ois

* chatServer의 socket과 serverSocket을 동기화한다. - socket.accept\( \)함수를 통과하면 server의 socket은 접속자 정보를 수집한다. - serverThread는 해당 접속자의 정보가 담긴 socket으로 oos, ois를 생성한다.
* 말하기, 듣기를 위해서는 반드시 해당 소켓객체가 생성되어있어야한다. - socket이 먼저
* oos : ObjectOutputStream\(client.getOutputStream\); - client정보가 담긴 socket으로 oos 생성
* ois : ObjectInputStream\(client.getInputStream\); - client정보가 담긴 socket으로 ois 생성
* client정보가담긴 server의 socket이 null상태라면 nullPointerException이 발생한다.

### ServerThread생성자

* serverthread에서 말하기는 send, broadCasting메서드가, 듣기는 run함수가 처리한다.
* 생성자의 역할 - 접속과 동시에 입장방송을 한다. : 로그인처리 - 단톡방 생성시 벡터에 접속자 thread를 추가한다. : 멀티스레드제어

### ServerThread run메서드

* Server의 run메서드 안에서 접속자 접속시 thread를 부여하고, 생성자를 호출한뒤 호출된다.
* 입장이 정상적으로 이루어져야 소통이 이루어진다.

### BroadCasting, send메서드

* broadCasting - 멀티스레드 말하기용 - 조건 없이 Vector안의 모든 접속자에게 말한다.
* send - 싱글스레드 말하기용 - 말하는 상대의 조건이 있는 경우에 사용한다.

### 접속자 정보 유지

1. DB, Oracle 연동
2. server에 멤버변수로 구분가능한 변수를 둔다. : nickName
3. 접속 변수마다 thread를 부여한다.
4. Vector에 담아 관리, 유지한다.

### 객체주입\(인스턴스화\)

* server - serverThread가 server의 안에 주입
* client - clientThread가 client의 안에 주입 - client가 login의 안에 주입 - pictureMessage가 client의 안에 주입 되어야한다.

## 이모티콘 구현

### PictureMessage

1. 이모티콘선택 화면을 띄워주는 클래스
2. 버튼과 이모티콘을 배열로 선언하여 버튼에 이미지를 붙인다. - setIcon\( \)함수 사용
3. 선택한 이모티콘 값이 유지되어 server - serverThread - clientThread 까지 가야 한다. - 값을 담을 멤버변수 선언
4. 이모티콘은 일반대화와 같이 처리 되면 될 것이다. - 일반대화와 같이 처리하기 위해서는 기본값이 필요하다. - 멤버변수에 String으로 기본값을 준다.
5. actionPerformed에서 선택한 버튼에 따라 멤버변수 값이 해당 이모티콘으로 초기화되어야한다.

### ChatClient

* 
### ChatServerThread

* 
### ChatClientThread

* 
후기 : 이제 추석연휴다. 재충전의 시간을 갖고 체력도 비축하고 그동한 했던 정리에 부족한 부분들을 메꾸는 시간을 가지도록 해야겠다.

