---
description: 2020.07.29 - 1일차에 배운것들 입니다.
---

# 1 Days - 설치 및 Java 자료형

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261 : Oracle.com
* 사용Tool : Eclipse : Eclipse.org

### 설치 및 확인

* JDK 설치
* Eclipse 설치
* CMD : java-version
* Eclipse 실행후 text encoding &gt; UTF-8로 설정

### 개념

* 개발자가 자바로 프로그래밍 &gt; Eclipse로 컴파일 &gt; 서버에 소스를 배포한다.
* 자가진단, 단위테스트를 통해 버그 및 정보를 얻는다.
* JDK - class 선언 &gt; 메인 메소스 선언
* class 선언 : 타입 변수이름;  : 개발자가 정하는 클래스를 정의하는 명사
* 메소스 선언 : 리턴타입 이름\(파라미터\); : 사용자와 소통하는 동
* void : 비어있다. void main = 리턴 값이 비어다.

### 예

* 사용자가 1,000원을 넣고 콜라를 눌렀다.
* \(1,000원 + 콜라버튼\)  = 메소드
* 콜라가 나왔다 = 리턴 값 \(초기화\)
* class가 명사이므로 디폴트가 달라지면 결과가 달라지기때문 초기화가 중요하다.

### 자료형 int

```text
int a=1;
int b=2;
system.out.print=(a+b); //출력 3
int c=(a-b);
system.out.print=(c); // 출력 3-1
```

```text
int a=1;
int b=2;
int c=(a-b);
system.out.println(a+b+c); 
system.out.print(5);
// 출력
// 2
// 5
```

| 자료 | 의 |
| :--- | :--- |
| int | 정 |

### 버그 확인하기.

Breakpoint에 bookmark추가 후 Debug.As &gt; 1Java Application &gt; switch &gt; F6

