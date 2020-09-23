---
description: 2020.09.22 - 30일차
---

# 30 Days - Talk - TimeLine, Server, Socket,

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261 : Oracle.com
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5

## 복습

### Thread 구현

* 방법1 : extends Thread - 자바는 단일상속만 허용한다.
* 방법2 : implements Runnable - 다중구현이 가능하다.
* Thread클래스의 run메서드를 오버라이딩해서 사용하기 위함이다.
* 서버쪽은 여러 접속을 처리해야하므로 thread가 있어야하고, 접속자의 입장에서도 병원-원무과-약국 순서로 가듯이 순서대로 서버에 접속해야하므로 thread가 필요하다.

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
* run\( \) 호출 : strat\( \);
* run\( \) 종료 : stop\( \);

### 메서드 접근제한자

* public &gt; protect &gt; friendly &gt; private

## JAVA

### 멀티스레드 구현시 주의사항

* dead lock : 교착상태가 일어나 계속 기다리게되는 것 - wait메서드 사용시 발생하지 않도록 주의해야한다.
* synchronzation : 동기화

## TalkServer&TalkClient

### Server기동 순서

1. ServerSocket 생성  - TalkSever클래스
2. Client 입장 - TalkClient클래스 - Client가 입장하려면 서버의 ip, port번호가 필요하다.
3. Socket생성 - TalkServer클래스 - Socket이 null이 아니게되는 시점 - Client가 입장하는 순간 주소번지를 갖는다.

### Socket의 갯수

1. 서버가 서버 port번호를 파라미터로 하는 ServerSocket을 생성한다. --소켓1
2. 클라이언트가 서버 port번호, 서버ip를 파라미터로 하는 Socket을 생성해 서버에 접속한다. --소켓2
3. ServerSocket과 접속자 Socket 사이에 OIS, OOS기능을 가진 Socket이 생성된다. --소켓3 - 이 Socket은 ServerSocket의 클라이언트 정보를 클라이언트 Socket에 전달한다. - ServerSocket과 클라이언트Socket사이에서 데이터가 유지, 기억되어야 하므로 필요한 것이다. - 접속자가 n명이면 Socket의 주소번지도 n개가 된다.
4. TalkServerThread클래스에서 듣기와 말하기가 진행된다.

### 접속순서

1. TalkServer실행=ts.initDisplay화면 실행
2. ts.run메서드 실행 및 대기 = 서버 대문 열어놓기
3. Client실행 = c.initDisplay화면 실행, c.init메서드 실행 - TalkClilent클래스는 접속자 한명만을 처리 하므로 run메서드를 사용하지 않는다.
4. c.init과 ts.run의 접선 및 TalkServerThread생성자 실행 - 접속이 허가되면 값을 List에 담아 TalkServerThread에 넘겨준다.
5. TalkClientThread의 run메서드에 메세지를 전달한다.

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

