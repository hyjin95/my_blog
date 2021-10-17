---
description: 2021.05.21 - 금요일
---

# 가상드라이브

### 가상드라이브 만들기

* 기본드라이브에 가상드라이브로 만들 폴더 생성
* 명령 크롬포트(CMD)에서 기본드라이브 경로 지정

```
c:\> subst k: c:\doopis
```

* c드라이브 경로에서, c드라이브의 doopis폴더를 가상으로 k드라이브로 설정
* subst로 만든 가상드라이브는 재부팅 시 초기화되어 c드라이브의 doopis폴더로 되돌아간다.

### 시작프로그램에 가상드라이브생성 추가

* 재부팅 시마다 새로 가상드라이브를 생성하기위해서는 시작프로그램에 프로그램을 등록하면 된다.
* 메모장에 위 subst 명령어를 작성해 확장자 .bat으로 저장한다.\
  subst k: c:\doopis

![](<../.gitbook/assets/1 (140).png>)

* Win + R > 실행팝업
* shell:startup 명령어로 윈도우 시작프로그램 파일에 접근한다.\
   C:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp
* 만들어둔 bat파일을 startup폴더안에 저장한다.\
