---
description: 2020.09.23 - 31일차
---

# 31 Days - Thread-revel2, Tokenizer, swicth-default,

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261 : Oracle.com
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5

## 복습

### List

* List는 인터페이스이므로 new List로 생성될 수 없다. 구현클래스가 필요하다.
* 추상메서드는 { }, 실행문이 없다.
* Vector와 ArrayList가 있다.

### Thread

* 멀티스레드 : Vector     : TalkServer
* 싱글스레드 : ArrayList : TalkClient
* 서로 다른 모든 스레드들을 stack에 따로 저장하여 제어, 관리한다.

### if

* 양방향 서비스, 소통 함수

### run-start\( \)

* start함수와 run함수 사이에는 내부적으로 일어나는 작용이 있어 run메서드를 직접 호출해서 실행시키지 않는다.
* 호출 해주는 주체 : JVM

### Procedure

## TalkServer - level 1

### TalkServer-Thread

* TalkServer에서 서버소켓이 생성되고, 접속이 일어나 accept가 되기 전까지는 대기 상태에 빠진다.
* run메서드가 먼저 불리면, 화면은 실행되지 않을 수 있다.
* 따로 동시에 실행하기 위해 thread를 부여한다. - thread 1 : main-initDisplay\( \)   thread 2 : run\( \) - stack에 thread가 두 개 따로 저장된다. = 각자 관리하는 메모리 영역이 다르다.
* 모든 thread는 소유권\(key\)를 갖는다.

### 순서 도식화

![level 1 - 1](../../.gitbook/assets/sheet.png)

1. 서버소켓 생성 후 대기 --소켓1 - ServerSocket\(port\) : 자신에게 port번호를 부여해 대문을 열어둔다.
2. 접속 준비 후 대기 - 접속 준비 : accept - TalkClient에서 접속발생시 진행된다.
3. 클라이언트 소켓 생성 --소켓2 - Socket\(ip, port\) : 접속할 서버의 ip, port번호를 담는다.
4. 소켓2와 소켓1이 접속되면서 소통할수 있는 채널이 열린다. - 서버소켓에는 port가 두개 존재한다. - port1\(local port\) : 서버의 대문 - port2 : 클라이언트의 소켓과의 채널이 생성되는 port - port2번의 채널이 열리면 정보수집이 가능해지므로 서버가 클라이언트의ip, port번호 를 알 수있게되어 관리가 가능해진다.
5. 서버에서 클라이언트 정보를 담는 소켓을 생성한다. --소켓3
6. 클라이언트의 정보를 담은 소켓3을 리턴한다. - =accept가 진행되었다.
7. 정보를 담은 소켓3에 thread를 부여해서 oos, ois를 구현해주고 데이터를  지속, 유지시킨다. - 여러 Socket을 관리하기 위해 접속 Socket마다 주소번지가 다른 thread를 복제, 부여한다.
8. 소켓3과 소켓2 사이에 채널이 열린다.  - 소켓3은 oos.writeObject함수를 이용해 말한다.

### Socket의 oos, ois구현

![level 1 - 2](../../.gitbook/assets/sheet2.png)

* Server에서는 각각의 클라이언트 정보를 유지시켜야하고, 접속자에 따라 갯수가 유동적이여야 한다. - Vector



