---
description: 2020.09.28 - 33일차
---

# 33 Days -1대1대화, 대화명변경, 스타일변경, 반복문 탈출, trim, set&getValueAt, JTextPane, windowListener

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261 : Oracle.com
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5

## 복습

### level3 - data이동

1. TomatoClient -&gt; ActionPerformed  - 대화상대 선택 -&gt; oos.writeObject\( \);
2. TomatoServerThread -&gt; Thread부여 및 Vector에 데이터 유지 - 듣기 : 생성자 -&gt; ois.readObject\( \); - 들은 내용 말하기 : run메서드 -&gt;  broadCasting, send 메서드 -&gt; oos.wrtieObject\( \);
3. TomatoClientThread -&gt; client화면에 응답 - 듣 : run메서드 -&gt; ois.readObject\( \);

### 반복문 탈출

* if : return;
* for : break;
* switch, while : 라벨

## 필기

### trim

* 문자열.trim\( \);
* 해당 문자열 앞과 뒤에 공백이 있다면 앞, 뒤 공백만 제거해주는 함수
* 문자열 사이의 공백은 삭제되지 않는다.

### get&setValueAt

* 소유주.getValueAt\(값을 가져올 row, 컬럼의 좌표\) - 컬럼의 좌표에는 첫번째 컬럼의 로우라면 0, 두번째 컬럼의 로우라면 1,.....이 들어간다.
* 소유주.setValueAt\(넣을 값, 넣어질 row, 컬럼의 좌표\)

### JTextPane

* Text화면에서 부분 text에 스타일을 적용할 때 사용하는 클래스이다.

### styledDocument

* 글씨체, 여백 등과 같은 서식, 스타일을 사용할 수 있는 클래스

### insertString

* insertString\(시작위치, 메세지, 서식\(스타일\)\)

### windowListener

* 윈도우 이벤트를 담당하는 인터페이스
* 여러 추상메서드 중 필요한 메서드만 구현하려면 - windowAdapter 클래스 이용 - 추상메서드가 오버라이딩 되어있는 클래스, 이 클래스를 통해 필요한 메서드만 재정의해 사용한다.
* JFrame 사용시 화면의 X버튼으로 창을 닫으면 보기에는 닫히는것 같지만 JVM은 실제로 남아있다. - JVM에게는 아직 가동중인 프로세서 - setDefaultCloseOperation함수를 이용해 프로세서까지 닫는 기능이 필요하다.

{% page-ref page="undefined-1.md" %}

## 1 : 1 대화 구현

* ClientThread와 ServerThread의 run메서드에 protocol클래스의 구분번호를 사용해 연결한다.

### TomatoClient

1. 대화상대 선택하기  - actionListener인터페이스를 사용해JTable에서 선택로우 값을 가져온다. - dtm은 데이터 처리를, JTable은 action을 사용할 때 사용된다. - jtb.getSelectRow\( \); - 상대를 선택하지 않은 경우 : JOptionPane.showMessageDialog\( \)함수로 안내메세지를 띄운다.
2. 상대를 선택한 경우 - String변수에 JTable에서 반응한 로우의 dtm data를 가져온다. - String 변수이름 = \(String\)dtm.getValueAt\(row, 0\); - getValueAt함수의 리턴타입은 Object타입이므로 캐스팅 연산자르 이용해 형전환 한다. - 보낼 메세지를 입력받을 JDialog창을 띄운
3. 본인에게 대화걸기를 차단하는 경우 - 전역변수에 선언한 nickName을 활용한다. - String  비교연산자 : 비교값.equals\(비교값\);
4. 입력이 일어나면 oos.wirteObject\( \);가 사용된다. - 말하기 기능을 사용하려면 예외처리가 필수이다. - try 문 안에 oos.writeObject\( \);를 구현한다.
5. 대화 상대로 선택된 상대는 말하기가 이루어진 뒤 초기화되어야한다. - jtb.clearSelection\( \);

### TomatoServerThread

1. run메서드에서 듣고 broadCasting, send메서드로 말한다. - StringTokenizer함수로 구분자를 뽑아 switch-case문을 사용한다. - nextToken\( \)함수로 내보낼 값을 골라 필요한 메서드를 사용해 말한다. - 필요한정보 : 보내는사람, 받는사람, 메세지 - 받는사람은 globalList에서 뽑아와야한다. : for, if문 사용
2. 받는사람과 보내는사람의 화면에만 메세지가 출력되어야한다. - this.send\( \);
3. 1대 1대화이므로 send메서드를 활용한다.

### TomatoClientThread

1. Server에서 말하는 내용을 듣고 화면에 띄워준다.
2. run메서드안에서 switch-case문과 StringTokenizer함수를 이용해 구현한다.

{% page-ref page="1-1.md" %}

## 대화명 변경

### TomatoClient

1. 대화명 입력값 가져오기 - actionListener인터페이스의 actionPerformed추상 메서드를 구현한다. - 입력값이 null이거나 공백을 제외한 글자가 1자 이하인 경우 안내창을 띄운다. : trim함수이용 - afterName==null \|\| afterName.trim\( \).length\( \) &lt; 1
2. 대화명 입력값 말하기 - oos.writeObject\( \);

### TomatoServerThread

1. 현재 채팅방의 모든 참가자들에게 변경알림을 알린다. - broadCasting함수 사용
2. 해당 서버 thread의 멤버변수 nickName도 변경처리 한다.

### TomatoClientThread

1. JTable의 dtm데이터도 새로고침한다. - for문으로 dtm의 데이터와 입력받은 afterName과 비교한다. - dtm.getRowCount\( \)함수 for문 조건을 걸고 - dtm.getValueAt\( \)함수로 데이터를 변경해준다. - break; 로 for문 탈출
2. client클래스의 화면의 title을 변경한다.
3. client클래스의 멤버변수 nickName도 afterName의 값으로 초기화 한다. 

{% page-ref page="undefined.md" %}

## level4 - 채팅창 글자 스타일 변경

### ChatClient

1.  기존에 화면으로 구현한 JTextArea 함수는 글씨 부분 변경이 되지 않는다. 
2. JTextPane을 이용한다.
3. JcolorChooser, colorSelectionModel, ChangeListener클래스를 사용한다.
4. 기존 메세지 전달 부분에 스타일을 추가한다.

### ChatServerThread

1. 기존 메세지 전달 부분에 스타일을 추가해야한다.

### ChatClientThread

1. 글자에 스타일을 입히기위한 작업 - SimpleAttrubuteSet함수를 이용해 스타일을 작성, 적용시킨다. - insertString\(시작위치, 메세지, 서식\(스타일\)\)
2. 예외처리 필수 - e.printStackTrace\( \);

{% page-ref page="undefined-1.md" %}

후기 : 자바에서 화면을 작성하려니 너무 복잡한것같다 하지만 이 과정을 통해 메서드를 연결하고, 클래스를 연결하고, api를 보는 법을 많이 배워가는것 같다. 

