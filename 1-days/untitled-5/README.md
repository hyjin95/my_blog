---
description: 2020.10.08 - 37일차
---

# 37 Days - 네이버 서버 캡차API활용

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261 : Oracle.com
* 사용Tool  - Eclipse : Eclipse.org

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

## 네이버 캡차API 활용

### 학습목표

### 캡차 제공 클래스

* 캡차 키 발급 요청 클래스 : ApiExamCaptchaNkey - client ID와 client Secret을 넣어 출력하며 키 값이 출력된다.
* 캡차 이미지 요청  클래스 : ApiExamCaptchaImage - 키 값을 넣어 출력하면 캡차 이미지가 출력된다.
* 사용자 입력값 검증 요청 클래스 : ApiExamCaptchaNkeyResult - 사용자 입력값이 올바른 값인지 검증한 결과를 JSON형식으로 반환 - boolean타입 result변수로 반환

