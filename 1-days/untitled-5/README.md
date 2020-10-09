---
description: 2020.10.07 - 37일차
---

# 37 Days - 네이버 캡차API, NaverCaptChatTest, 쿼리스트링, 대기실&톡방 구현 마무리, emptyBorder

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261 : Oracle.com
* 사용Tool  - Eclipse : Eclipse.org

## 필기

### 쿼리스트링

* 네이버 서버에 검색을 요청\(통신\)할 때 주소창에 검색값이 추가되는 것을 볼 수 있다. - 파라미터로 사용자의 값을 받아오는 것과 비슷하다.
* URL이 요청해서 나온 값이 URL뒤에 String타입으로 붙는데, 이를 쿼리스트링이라 한다.
* **oos를 사용하지 않고 사용자의 입력값을 서버에 전송하는 방법**이다.
* 웹서버에 요청을 보낼때 주소뒤에 물음표를 붙이고, 변수이름 = 값&변수이름2 = 값2&변수이름3 = 값3 이런식으로 쿼리스트링을 작성한다.

{% page-ref page="undefined.md" %}

### createEmptyBorder\( \)

* Border클래스가 제공하는 BorderFactor.createEmptyBorder\( \)메서드
* 소유주의 공간을 일절 효과없는 빈 상태로 경계를 생성한다.

## 네이버 API사용준비

### 네이버

1. 네이버 로그인
2. 네이버 개발자 센터 페이지 - 서비스 API - 캡차
3. 오픈 API신청
4. 계정 본인 인증
5. 애플리케이션 등록\(API이용신청\) - 임의이름지정, 캡차\(이미지\), 서비스환경 : WEB
6. Eclipse에서 사용할 xml 선택, port번호 확인 -&gt; Server의 Modules탭
7. 만들어둔 도메인 index.jsp on server로 실행 후 상단의 주소복사 - 5번 서비스환경에 붙여넣기 - 제일 끝단의 파일 이름은 지우고 주소만 넣는다.
8. client ID, client Secret 배포, 기록해두기 

### Eclipse

1. window 
2. preferences
3. General
4. Workspace : Refresh using native hooks or polling 체크
5. build : save automatically before manual build 체크
6. Apply and close

## 네이버 캡차API 살펴보기

### 학습목표

* 외부 API를 활용하는 능력을 키운다.
* 내가 직접 스레드와 소켓통신을 하지 않아도 된다.
* 새로운 자료구조를 익힌다. - 웹 : json\(자바스크립트와 호환 - php, C\#, 파이썬, node.js, react, ...\) -&gt; 객체스러워지고 있다. - 오라클 같은 대형 솔루션은 사용하기 힘들어 nosql제품이 다수 출현한다. - 디바이스가 다양해지면서 자료구조처리를 할 줄 알아야 한다.
* json - 변수, 배열, 객체배열\(DeptVO\[ \]\), List&Map 등의 값을 웹에서 사용하려면, - JSON형식으로 변환해야 Web, 안드로이드 IOS에서 시스템을 만들때 사용할 수 있다.

### 캡차 제공 클래스

* 캡차 키 발급 요청 클래스 : ApiExamCaptchaNkey - client ID와 client Secret을 넣어 출력하며 키 값이 출력된다. - 소켓통신이 아닌 웹통신을 이용한다.
* 캡차 이미지 요청  클래스 : ApiExamCaptchaImage - 키 값을 넣어 출력하면 캡차 이미지가 출력된다.
* 사용자 입력값 검증 요청 클래스 : ApiExamCaptchaNkeyResult - 사용자 입력값이 올바른 값인지 검증한 결과를 JSON형식으로 반환 - boolean타입 result변수로 반환

{% page-ref page="navercaptchatest/apiexamcaptchankey.md" %}

{% page-ref page="navercaptchatest/apiexamcaptchaimage.md" %}

{% page-ref page="navercaptchatest/apiexamcaptchankeyresult.md" %}

## NaverCapchaTest - 캡차 새로고침 구현

### NaverCaptchaTest

* 화면구현 JFrame 상속
* 새로고침, 확인버튼의 이벤트 처리를 해야한다. - implements ActionListener
* 새로고침 구현을 위해 Container클래스가 필요하다. - Container클래스의 revalidate\( \)메서드 사용
* 사용자가 입력하는 JTextField가 필요하다.
* JLable 컴포넌트에 캡차 이미지를 표시한다. - 이미지 주소 필요
* 캡차제공클래스에서 필요한 클래스, 메서드를 끌어다 사용한다.

{% page-ref page="navercaptchatest/" %}

## 대기실&그룹방 구현 마무리

1. 그룹방 생성시 생성자 외의 대기실 접속자들의 화면에도 생성된 그룹방이 나타나야한다. - ChatClientThread의 run메서드안에 case ROOM\_LIST 구현 - Protocol.ROOM\_LIST : 방 목록
2. 그룹방에 입장시, 입장한 사람의 화면은 대화방으로 변경되고, 상태도 대화방이름으로 변경돼야한다. - ChatServerThread의 run메서드안에 case ROOM\_INLIST 구현 - Protocol.ROOM\_INLIST : 방 입장인원 목록 - MessageRoom클래스에서 말하는 내용이다.

{% page-ref page="and-+.md" %}

후기 : 채팅프로그램이 마무리 되었다! 배운것을 토대로 좀더 만져보고 구현해보는 시간을 가져야 겠다.

