---
description: 2020.09.25 - 32일차
---

# 32 Days - Tomato-level3, UI, setCarePosition, paint, setOpaque&Editable&lineWrap

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261 : Oracle.com
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5

## 복습

### Thread, Runnable 역할

* 한정된 자원을 나눠야 해서 경합이 일어날때 순서대로 대기, 진행할 수 있게 해주는 클래스,인터페이스
* run추상메서드를 Override해서 사용한다. - 소유주.strat\( \); : 소유주안의 run메서드 진행
* Thread가 제공해주는 multi Thread에서 사용되는 메서드 - sleep\( \) : 정보를 담는 동안 재운다. millisec = 1000 = 1sec - wait\( \) : 해당 스레드의 순서가 아니라면 대기시킨다. - notify\( \) : 대기하던 스레드의 순서가 오면 깨워준다. - notifyAll\( \) : 대기하던 스레드의 순서가 오면 모든 스레드를 깨워준다.
* dead lock - wait함수를 사용할때 발생할 수 있는 상태 - 순서가 와도 thread가 진행되지 않을때를 말한다.

### Thread, Runnable 인스턴스화

* Override되는 run메서드를 제외한 다른 메서드들을 사용하려면 인스턴스화를 해야한다.
* Thread, Runnable은 추상메서드가 있으므로 생성자로 호출, 생성이 불가능하다.
* 구현클래스로 생성을 해주어야한다. - Runnable r = 구현체클래스\( \); - Thread th = new Thread\(run메서드를 구현한 클래스명 \);   th.sleep\( \);

### 예외처리

* try-catch
* 서버접속시 접속오류와 같이 오류가 발생할 소지가 있는 함수나 수식을 사용할 때에는 반드시 예외처리를 해주어야 한다. - ex\) int i, 5/i라는 수식을 사용할때에도, i에 0이올수 있으니 예외처리가 필요하다.

### Server의 port

1. local port : 서버의 port값, 서버쪽 대문
2. port : 외부사용자의 port값, 사용자쪽 대문

## TomatoServer - Level3

### setCaretPosition

* 스크롤바가 있는 입력, 출력화면에서 입,출력이 화면을 넘어갔을 때, 커서가 이 값을 따라가서 자동으로 스크롤을 내려주는 함수

{% page-ref page="setcareposition.md" %}

### 직관적인 이름 사용하기

* Band클래스에서 메세지를 구분할때 사용하던 번호 100, 200,...를 더 직관적인 이름을 부여한다.

{% page-ref page="protocol.md" %}

### paint

* paint함수는 컴포넌트에 사용자가 직접 그림을 그릴 수 있게 해주는 함수이다.
* 그림을 그리고자 하는 객체안에 paint를 오버라이딩 하여 사용한다.
* 파라미터로 Grapics타입의 변수를 하나 갖는다.
* paint메서드 안에 위치, Graphics타입을 설정한다.

### Graphics

* 화면 그리기에 필요한 그리기 메서드를 갖고 있다. - 직선, 사각형, 글자, 이미지

### setOpaque&editable&lineWrap

* 모두 boolean타입 함수
* 소유주.setOpaque\( \); - 소유주의 배경을 투명하게\(false\) 또는 불투명하게\(true\) 설정한다. - true로 적용되어야 색상지정이 가능하다.
* 소유주.setEditable\( \); -  사용자가 소유주 부분에 텍스트를 입력할수 없게\(false\) 또는 있게\(true\) 설정한다.
* 소유주.setLineWrap\( \); - 소유주 부분에 텍스트 입, 출력시 자동 줄바꿈 기능을 가능하게\(true\) 또는 불가능하게\(false\) 설정한다.

{% page-ref page="tomatoclientthread-img.md" %}

후기 : 다음주는 추석이 있어 이틀만 수업을 듣는데 2주동안 자리별로 격일로 비대면 수업을 진행하기로 했다. 집에서도 집중하기!

