# 34 Days - level4복습, 이모티콘구현,

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

1. 
