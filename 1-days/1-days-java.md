---
description: 2020.07.29 - 1일차
---

# 1 Days - 설치 및 Java 자료형

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261 : Oracle.com
* 사용Tool : Eclipse : Eclipse.org

### 설치 및 확인

* JDK 설치
* Eclipse 설치
* CMD : java -version
* Eclipse 실행후 text encoding &gt; UTF-8로 설정

### 개념

* 개발자가 자바로 프로그래밍 &gt; Eclipse로 컴파일 &gt; 서버에 소스를 배포한다.
* 자가진단, 단위테스트를 통해 버그 및 정보를 얻는다.
* JDK - class 선언 &gt; 메인 메소드 선언
* class 선언 : 타입 변수이름;  : 개발자가 정하는 클래스를 정의하는 명사
* 메소드 선언 : 리턴타입 이름\(파라미터\); : 사용자와 소통하는 동사
* void : 비어있다. void main = 리턴 값이 비어있다.

### 예시

* 사용자가 1,000원을 넣고 콜라를 눌렀다.
* \(1,000원 + 콜라버튼\)  = 메소드
* 콜라가 나왔다 = 리턴 값 \(초기화\)
* class가 명사이므로 디폴트가 달라지면 결과가 달라지기때문 초기화가 중요하다.

### 자료형 int

```java
int a=1;
int b=2;
system.out.print=(a+b); //출력 3
int c=(a-b);
system.out.print=(c); // 출력 3-1
```

```java
int a=1;
int b=2;
int c=(a-b);
system.out.println(a+b+c); 
system.out.print(5);
// 출력
// 2
// 5
```

```java
package book.ch2;
//가상머신과의 연결고리가 끊어 졌으므로 다시 처음부터 실행할 수 없다.
public class Tivoli {

	public static void main(String[] args) {
		int first=1;
		int second=2;
		//파라미터 두 개짜리 메소드가 정의되어 있지 않아서 컴파일 
		//에러가 발생
		//이 상태에서는 실행해 볼 수가 없다. 
		//실행하려 해도 Tivoli.class파일이 만들어지지 않으므로
		System.out.println(first+second);
		//println(first,second)이면, println메소드에는 
		//파라미터 두개를 정의하고 있지 않거나 타입이 같지않기때문에 
		//실행되지 않는다.
		//printhln메소드는 JDK에 내장되어있는 메소드이다.
		System.out.print(5);
	}

}

```

* 자료형 : 예를 들어 사람의 나이를 저장하려면, 나이는 정수, 이름은 문자 형태를 사용해  야 하는데, 이 형태를 자료형이라고 부른다. 
* 변수를 선언한다 : 개발자가 자료형을 선택해서 변수이름을 정하는 것을 '변수를 선언한다.'라고 한다.

| 자료 | 의미 |
| :--- | :--- |
| int | 정수 |

int는 정수만 담기때문에 3.14를 대입하면 0.14는 overflow된다.

### 버그 확인하기

Breakpoint에 bookmark추가 후 Debug.As &gt; 1Java Application &gt; switch &gt; F6

후기 : 첫 날이라 따라가기 바빴지만, 시간이 갈수록 질문 및 여러가지를 더 시도할수 있는 개발자가 되 싶습니다.

