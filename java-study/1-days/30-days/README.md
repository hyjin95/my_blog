---
description: 2020.09.22 - 30일차
---

# 30 Days - Talk - TimeLine, Server, Socket, Thread, oos, ois, run

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261 : Oracle.com
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5

## 복습

### Thread 구현

* 방법1 : extends Thread - 자바는 단일상속만 허용한다. - run메서드를 오버라이딩 하여 사용가능
* 방법2 : implements Runnable - 다중구현이 가능하다. - 인터페이스 이므로 인스턴스화가 불가능 하기때문에 구현 오브젝트를 Thread생성자의 파라미터로 넘겨야 run메서드를 사용가능 - Thread의 생성자중에 Thread\(Runnable target\)을 사용 - Runnable target = 시작을 적용할 run메서드를 가진 객체
* Thread클래스의 run메서드를 오버라이딩해서 사용하기 위함이다.
* 서버쪽은 여러 접속을 처리해야하므로 thread가 있어야하고, 접속자의 입장에서도 병원-원무과-약국 순서로 가듯이 순서대로 서버에 접속해야하므로 thread가 필요하다.

{% page-ref page="untitled-3.md" %}

### 메서드 오버라이딩

* @Override
* 파라미터와 타입이 같아야한다.
* 기능을 재정의해서 사용한다.

### RUN\( \)

* 여러 접속이 일어날때, 순서대로 진행하게 한다.
* wait\( \) : Thread를 대기시킨다.
* notify\( \) : 대기하던 single thread를 차례가 되어 호출한다.
* notifyAll\( \) : 대기하던 모든 thread를 호출한다.
* sleep\( \) : 일정시간 얼린다. 1000=1 
* run\( \) 호출 : strat\( \); - 직접 run메서드를 호출하지 않는다. - start함수와 run메서드 사이에 내부적으로 일어나는 작용이 있다. - 호출해주는 주체 : JVM
* run\( \) 종료 : stop\( \);

### 메서드 접근제한자

* public &gt; protect &gt; friendly &gt; private

## JAVA

### 멀티스레드 구현시 주의사항

* dead lock : 교착상태가 일어나 계속 기다리게되는 것 - wait메서드 사용시 발생하지 않도록 주의해야한다.
* synchronzation : 동기화

## TalkServer&TalkClient : 서버만들기

### Server기동 순서

1. TalkServer클래스 : ts.initDisplay화면 실행, ServerSocket 생성 
2. ts.run메서드 실행 및 대기 = 서버 대문 열어놓기
3. TalkClient클래스 :  c.initDisplay화면 실행, c.init메서드 실행

   - TalkClilent클래스는 접속자 한명만을 처리 하므로 run메서드를 사용하지 않는다.

   - Client가 입장하려면 서버의 ip, port번호가 필요하다.

4. c.init과 ts.run의 접선 및 TalkServerThread생성자 실행 - 접속이 허가되면 값을 List에 담아 TalkServerThread에 넘겨준다. - Socket생성 - TalkServer클래스 - Socket이 null이 아니게되는 시점 - Client가 입장하는 순간 주소번지를 갖는다.
5. TalkClientThread의 run메서드에 메세지를 전달한다.

### Socket의 갯수

1. 서버가 서버 port번호를 파라미터로 하는 ServerSocket을 생성한다. --소켓1
2. 클라이언트가 서버 port번호, 서버ip를 파라미터로 하는 Socket을 생성해 서버에 접속한다. --소켓2
3. ServerSocket과 접속자 Socket 사이에 OIS, OOS기능을 가진 Socket이 생성된다. --소켓3 - 이 Socket은 ServerSocket의 클라이언트 정보를 클라이언트 Socket에 전달한다. - ServerSocket과 클라이언트Socket사이에서 데이터가 유지, 기억되어야 하므로 필요한 것이다. - 접속자가 n명이면 Socket의 주소번지도 n개가 된다.
4. TalkServerThread클래스에서 듣기와 말하기가 진행된다.

### Talk 클래스 정리

<table>
  <thead>
    <tr>
      <th style="text-align:left">TalkServer</th>
      <th style="text-align:left">TalkServerThread</th>
      <th style="text-align:left">TalkClient</th>
      <th style="text-align:left">TalkClientThread</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">
        <p>JFrame, Runnable</p>
        <p>&#xC811;&#xC18D; &#xB85C;&#xADF8; &#xCD9C;&#xB825;&#xC6A9;</p>
        <p>main,&#xD654;&#xBA74; &#xAD6C;&#xD604; &#xBA54;&#xC11C;&#xB4DC;</p>
      </td>
      <td style="text-align:left">Thread &#xC0C1;&#xC18D;</td>
      <td style="text-align:left">
        <p>JFrame, ActionListener</p>
        <p>&#xC0AC;&#xC6A9;&#xC790; &#xC811;&#xC18D;&#xC6A9;</p>
        <p>main,&#xD654;&#xBA74; &#xAD6C;&#xD604; &#xBA54;&#xC11C;&#xB4DC;</p>
      </td>
      <td style="text-align:left">Thread &#xC0C1;&#xC18D;</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p>&#xC778;&#xC2A4;&#xD134;&#xC2A4;&#xD654;</p>
        <p>TalkServerThread(this)</p>
      </td>
      <td style="text-align:left">
        <p>&#xC778;&#xC2A4;&#xD134;&#xC2A4;&#xD654;</p>
        <p>TalkServer&#xC548;&#xC5D0; &#xD638;&#xCD9C;</p>
      </td>
      <td style="text-align:left">
        <p>&#xC778;&#xC2A4;&#xD134;&#xC2A4;&#xD654;</p>
        <p>TalkClientThread(this)</p>
      </td>
      <td style="text-align:left">
        <p>&#xC778;&#xC2A4;&#xD134;&#xC2A4;&#xD654;</p>
        <p>TalkServer&#xC548;&#xC5D0; &#xD638;&#xCD9C;</p>
      </td>
    </tr>
  </tbody>
</table>

* TalkClientThread : 말하기 -&gt; TalkServerThread : 듣기 
* TalkClient : JFrame을 상속 -&gt; View가 있는 클래스이다 -&gt; event를 감지하는 클래스이다.  -&gt; actionPerformed = 말한다. -&gt; timeServer의 run = 듣는다. -&gt; run = 응답한다.

### TalkServerThread의 인스턴스화 위치

* TalkServer클래스의 run메서드 accept다음 - TalkServerThread tst = new TalkServerThread\(this\); - 사용자의 접속 후, 듣고나서야 사용자에게 말하기를 할 수 있다.
* 인스턴스화 후, Thread를 시작한다. - tst.start\(\);

### List-Vector 의 필요성

* n명이 접속한다 -&gt; 방이 n개 필요하다.
* TalkServerThread는 모든 접속자에 대한 정보를 담는다. - Vector&lt;TalkServerThread&gt; - v.size = 전체 접속자 수
* TalkServerThread에서 메세지를 전달하려면 TalkServer의 Socket으로 ois, oos를 만들어야한다.
* TalkServer는 thread를 화면과 접속 둘로 분리하고, run메서드로 경합,대기,지속을 처리한다.
* tst에서는 broadCasting:멀티스레드용, send:싱글스레드용 메서드로 메세지를 전달한다.
* broadCasting메서드는 v.size만큼 send메서드를 반복한다.

### Vector 타입 = TalkServerThread

* tst클래스는 TalkServer클래스의 run메서드 안에서 n명의 client가 접속 때마다  인스턴스화 되어 n개의 주소번지를 갖는다. = for문
* Vector에 담아야하는 것은 client 한명의 thread가 아닌 모든 접속자의 thread를 담아야한다.
* ClientThread와 달리 TalkServer는 while문에서 tst를 인스턴스화 하여 주소번지가 계속 변경된다.
* ClientThread : single thread
* TalkServerThread : multi thread

### 입장 알림 구현

1. 로그인이 성공하는 순간, 현재 접속자에게 채팅방 입장 메세지를 출력한다.
2. '나'에게는 입장해있는 모든 접속자를 띄워준다. 접속자 모두에게 '나'의 입장 메세지를 띄운다.
3. Client클래스에서 서버접속과 함께 100\#닉네임 메세지를 말한다. - oos.writeObject\(\);
4. server클래스에서 접속이 성공하면 thread를 부여하고 socket을 생성한다.
5. severThread클래스에서 접속된 thread의 말하기를 듣고, 다시 말한다. - ois.readObject\( \); - StringTokenizer - '나'에게 send메서드를 이용해 메세지를 말하고, 모두에게는 broadCasting메서드를 이용해 말한다. - 접속과 동시에 이루어져야 하므로 생성자에 구현한다.

{% page-ref page="talkserverthread-1.md" %}

{% page-ref page="talkclient.md" %}

{% page-ref page="talkserverthread.md" %}

후기 : 날씨가 추워지고, 코로나는 아직도 기승을 부리지만 열심히 하자! 이제 채팅방을 구현할 수 있는 socket과 thread진도를 나간다. 특히 복습을 잘 해서 수업을 따라가야겠다.

