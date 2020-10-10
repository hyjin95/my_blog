---
description: 2020.10.07 - 37일차
---

# 37 Days - 캡차소스,NaverCaptChatTest, 쿼리스트링, 대기실톡방 마무리, emptyBorder, revaildate, HTTP, URL, cookie, mime

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261 : Oracle.com
* 사용Tool  - Eclipse : Eclipse.org

## 필기

### createEmptyBorder\( \)

* Border클래스가 제공하는 BorderFactor.createEmptyBorder\( \)메서드
* 소유주의 공간을 일절 효과없는 빈 상태로 경계를 생성한다.

### revalidate\( \)

* Container 제공 메서드
* 새 구성요소가 추가되거나 이전 요소가 제거되면 컨테이너에서 호출된다.
* 이 호출은 레이아웃 관리자에게 새 구성요소 목록을 기반으로 재설정하도록 지기하는 명령이다.

### 쿼리스트링

* 네이버 서버에 검색을 요청\(통신\)할 때 주소창에 검색값이 추가되는 것을 볼 수 있다. - 파라미터로 사용자의 값을 받아오는 것과 비슷하다.
* URL이 요청해서 나온 값이 URL뒤에 String타입으로 붙는데, 이를 쿼리스트링이라 한다.
* **oos를 사용하지 않고 사용자의 입력값을 서버에 전송하는 방법**이다.
* 웹서버에 요청을 보낼때 주소뒤에 물음표를 붙이고, 변수이름 = 값&변수이름2 = 값2&변수이름3 = 값3 이런식으로 쿼리스트링을 작성한다. - String apiURL = "https://openapi.naver.com/v1/captcha/nkey?code=" + code + "&key=" + key + "&value=" + value;
* URL에 정보를 담으므로 해당 페이지의 정보가 외부에 노출될 수 있다. - 중요한 정보는 절대로 쿼리스트링으로 내보내지 않는다. - 스크립트 코드도 노출되지 않도록 외부에 별도 작성하여 import로 사용한다.   XXX.js, XXX.css - 경로의 경우도 절대경로가 노출되면 서버정보가 외부에 노출되는 것이므로 상대경로를 사용한다.    상대경로를 사용해면 전체경로를 알수 없으므로

### http 프로토콜

* Hypertext Transfer Protocol - 하이퍼텍스트 기반으로 데이터를 전송\(transfer\) = 링크를 기반으로 데이터에 접속한다.
* 인터넷상에서 데이터를 주고 받기 위한 서버, 클라이언트 모델을 따르는 **전송 프로토콜**
* 어떤 종류의 데이터든지 전송할 수 있다. - HTML문서, 이미지, 동영상, 오디오, 텍스트 문서, ....
* 클라이언트에서 요청\(request\)를 보내면 서버는 요청을 처리해서 응답\(response\)한다.
* 클라이언트 - 클라이언트 소프트웨어가 설치된 컴퓨터에서 URL을이용해 서버에 접속, 데이터를 요청한다. - 서버에 요청하는 클라이언트 s/w : IE, Chrome, Firefox,.....
* 서버 - 클라이언트의 요청을 받아서 응답 소프트웨어가 설치된 컴퓨터를 말한다. - 요청을 해석하고 응답하는 s/w : Apache, Ngins, IIS, lighttpd,.....
* 웹서버의 표준포트 : 80번 으로 서비스한다.
* Connectless방식으로 작동한다. - 서버에 연결하고 응답을 받으면 연결을 끊는다. - 접속유지를 최소화하므로 불특정 다수를 대상으로하는 서비스에 적합하다. = 브라우저 - 연결을 끊어버려 클라이언트의 이전상태를 알 수 없다. = stateless   로그인을 성공햇더라도 정보를 유지 할 수 없다. = cookie를 이용해 해결하고있다.
* 제공 메서드 - GET        : 정보 요청         = SELECT - POST     : 정보 밀어넣기 = INSERT - PUT        : 정보 업데이트 = UPDATE - DELETE : 정보 삭제          = DELETE

### cookie

* 클라이언트와 서버의 상태정보를 담고있는 정보조각
* 클라이언트가 로그인에 성공한다면, - 서버는 로그인 정보를 자신의 데이터베이스에 저장하고 - 동일한 값을 cookie의 형태로 클라이언트에 보낸다.
* 첫 요청 : 서버는 cookie를 key로하는 값을 DB에 저장하는 방식으로 "세션"을 유지하고 클라이언트에게 cookie를 반환한다.
* 두번째 요청 : 클라이언트는 다음 요청때 cookie를 서버에 보내 서버가 그 값으로 DB를 조회해 로그인여부를 확인하는 것이다.

### URL

* Uniform Resource Identfiers
* World Wide Web상에서 클라이언트 s/w에게 접근하고자 하는 자원의 위치를 찾아주는 프로토콜이다. - 자원 : HTML문서, 이미지, 동영상, 오디오, 텍스트문서, .....
* http://www.naver.com/index.php URL이 있을때, - http : 자원에 접근하기 위해 http프로토콜을 사용한다. - www.naver.com : 자원의 인터넷상 위치. 도메인은 ip주소로 변환되어 서버의 위치를 찾을 수 있다. - index.php : 요청할 자원의 이름 - **프로토콜, 위치, 자원명**으로 접근이 가능한 것이다.

### mime 파일변환타입

* Multipurpose Internet Mail Extensions
* 현재는 웹을 통해서 여러형태의 파일을 전달하는데 사용된다. - 이메일과 함께 동봉할 파일을 텍스트 문자로 전환해서 이메일 시스템을 통해 전달하기 위해 개발
* **인코딩\(Encoding\)** : 텍스트파일로 변환
* **디코딩\(Decoding\)** : 텍스트파일을 바이너리 파일로 변환
* MIME로 인코딩한 파일은 Content-type정보를 파일의 앞부분에 담는다. - Multipart Related MIME타입 - XML Media 타입 - Application 타입 - 오디오 타입 - Multipart 타입 - TEXT 타입 - file 타입\
* 브라우저는 마임타입을 통해서 해당 페이지에 대한 해석을 하게된다. - 태그는 해석하고 내용만 출력해준다. - 인터프리너틔 역할을 브라우저가 한다.
* 마임타입이 선언되어 있어야 브라우저가 알맞은 해석을 할 수 있다.
* mime type : 메인타입/서브타입  - text/html text/xml text/javascript = 마임타입

{% page-ref page="undefined.md" %}

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

### 클래스 소스분석

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

1. 그룹방 생성시 생성자 외의 대기실 접속자들의 화면에도 생성된 그룹방이 나타나야한다. - ChatClientThreadVer2의 run메서드안에 case ROOM\_LIST 구현 - Protocol.ROOM\_LIST : 방 목록
2. 그룹방에 입장시, 입장한 사람의 화면은 대화방으로 변경되고, 상태도 대화방이름으로 변경돼야한다. - ChatServerThread의 run메서드안에 case ROOM\_INLIST 구현 - Protocol.ROOM\_INLIST : 방 입장인원 목록 - MessageRoom클래스에서 말하는 내용이다.

{% page-ref page="and-+/" %}

후기 : 채팅프로그램이 마무리 되었다! 배운것을 토대로 좀더 만져보고 구현해보는 시간을 가져야 겠다.

