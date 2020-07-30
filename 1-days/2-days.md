---
description: 2020.07.30 - 2일차
---

# 2 Days -

### 1일차 복습 질문

1. JDK란  : Java Development Kit의 약어로 Back-end에서 사용하는 Java언어이고, 일꾼 역할을 하는 소프트웨어적인 기계이다. Eclipse에서  java를 class로 컴파일링 하는데 사용된다.
2. 파라미터란 : 메소드 선언에서 \( \)안에 들어가는 값으로, 1~n개까지 들어갈 수 있다.
3. int한 : 정수를 담는 변수 타입이다.

### 진도

```java
package book.ch2;
//에러에는 두가지가 있다.
//문법에러
//런타임에러(실행에러:문법에러는 없다.)
public class Variable1 {

	public static void main(String[] args) {
		int i = 3;
				System.out.println(3);
				System.out.println(i);//여기서  출력되는 3과 3은 다르다. 3은 그냥 숫자고 i는 변수=값 이다.
				String name = "KOSMO";
				name="IT사관학교";//초기화
				System.out.println("KOSMO");
				System.out.println("KOSMO");
				System.out.println("KOSMO");
				System.out.println("====================");
				System.out.println(name);
				System.out.println(name);
				System.out.println(name);
			
	}

}

```

```java
package book.ch2;

public class Method1 {
	int i =3;
	void methodA() { //메소드를 선언
		System.out.println("methodA 호출 성공");//실행문
	}//end of methodA-다시 나를 부른곳으로 돌아감
	/* 실행 순서 11-12-13-5-6-7-14 항상 메인 메소드 부터 실행된다.
	 * 
	*/ 	
	public static void main(String[] args) {
		Method1 m1 = new Method1();//클래스의 인스턴스화
		m1.methodA();//내안에 있는 메소드 호출
		System.out.println("methodA 호출 성공");
	}//main메소드가 없으면 이 클래스는 실행되지않는다. methodA라는 명령어만 가지고 있을뿐이다.

}

```

