---
description: 2020.07.30 - 2일차
---

# 2 Days - 인스턴스화, 변수, 초기화

### 1일차 복습 질문

1. JDK란  : Java Development Kit의 약어로 Back-end에서 사용하는 Java언어이고, 일꾼 역할을 하는 소프트웨어적인 기계이다. Eclipse에서  java를 class로 컴파일링 하는데 사용된다.
2. 파라미터란 : 메소드 선언에서 \( \)안에 들어가는 값으로, 1~n개까지 여러개 들어갈 수 있다.
3. int란 : 정수를 담는 자료형\(타입\)이다.

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261 : Oracle.com
* 사용Tool : Eclipse : Eclipse.org

### 학습목표

* 변수를 선언할 수 있다. \[핵심키워드-선언\]
* 클래스 안에서 변수와 메소드를 구분하여 말할 수 있다. \[핵심키워드-구분\]
* 선언한 변수에 적당한 값으로 초기화를 할 수 있다. \[핵심 키워드-초기화\]
* 초기화의 위치가 결과값에 어떠한 영향을 주는지 설명할 수 있다. \[핵심 키워드-위치\]

### 인스턴스화와 순서

```java
package book.ch2;

public class Method1 {
	int i = 3;
	void methodA() { //메소드를 선언
		System.out.println("main 호출 성공");//실행문
	}//end of methodA-다시 부른곳으로 돌아감

	public static void main(String[] args) {
		Method1 m1 = new Method1();//클래스의 인스턴스화
		m1.methodA();//클래스 안에 있는 메소드 호출
		System.out.println("methodA 호출 성공");
	}//main메소드가 없으면 이 클래스는 실행되지않는다. methodA라는 명령어만 가지고 있는 것이다.
}

```

* method의 **위치**에 주의하자
* 실행 순서 : 9-10-11-5-6-7-12 : 메인 메소드 부터 실행된다.
* 10번 : Method1.java 라는 클래스를 m1이라는 주소를 이용해 RAM에 저장시킨다.\(=인스턴스\)
* 11번 : m1.methodA를 호출한다. m1은 주소 역할을 한다.
* 5번 : 11번에서 methodA를 호출하였으므로 methodA가 선언된 곳으로 간다.
* 출력 : main 호출 성공 / methodA 호출 성공
* main 메소드가 없다면 실행되지 않는다.

### 변수선언과 초기화

```java
package book.ch2;
//에러에는 두가지가 있다.
//문법에러
//런타임에러(실행에러:문법에러는 없다.)
public class Variable1 {

	public static void main(String[] args) {
		int i = 3;
				System.out.println(3);
				System.out.println(i);//여기서  출력되는 3과 3은 다르다. 3은 그냥 숫자고 i는 변수(=값) 이다.
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

* 변수를 **초기화** 해보자
* 변수를 사용하는 이유 : 일괄처리
* i값을 다른 정수로 선언하면, 출력되는 값도 달라진다. 
* 출력결과 : KOSMO / KOSMO / KOSMO / =================== / IT사관학교 / IT 사관학교 / IT사관학교

```java
package book.ch2;

public class Quiz1 {

	public static void main(String[] args) {
		int a; //a는 정수다. 선언
		a=5; //정수타입의 a에 5를 담았다. 재사용을 위한 초기
		int b; //또 다른 변수 b를 정수로 선언했다.
		b=a; //변수 b에 a에 담긴값을 대입했다. =는 대입연산자다. 
		a=10;
		System.out.println("변수 a는"+a+"입니다.");//바뀌지않는다.
		System.out.println("변수 b는"+b+"입니다.");//b는 15번에서 값이 결정되었다.
	}

}
```

* 변수 : 변할수 있는 값
* 변수의 **위치**에 주의하자
* 출력결과 : 10 / 5
* 9번에서 b는 a의 원래 값인 5로 결정되었다. \(=는 "같다"가 아니라 대입연산자이다.\)
* 10번에서 a 바뀌지만  b 이미 결정되었기때문에 영향을 받지않는다.
* "같다" : ==

```java
package book.ch2;

public class Quiz4 {

	public static void main(String[] args) {
		int tot = 0; //0으로 초기화
		int a = 1;
		for(int i=0;i<10;i=i+1) {
			//System.out.println(i);
			tot = tot+a; //tot=0+1, tot=1+1, tot=2+1
			//System.out.println(tot+a);
		  //System.out.println(tot);//1,3,6,~55 =10번 반복한다.
		    a = a+1;			
		}
		System.out.println(tot);//55 =for문 밖이기때문에 최종 값만 도출된다.

	}

}

```

* tot의 **초기화**에 주의하자. 결과 값이 달라질 수 있다.
* 6-7번 : tot의 시작 값은 0이고, a의 값은 정수 1이다. 
* 8번 : i는 0이상 10미만의 값이면 +1 되며 반복한다. \(=10번 반복한다.\)
* 9번 : 결과 도출 :  0 / 1 / 2 / 3 / 4 / 5 / 6 / 7 / 8 / 9 \(10번 반복하는 동 +1하면서 출력한다.\)
* 10번 :  tot값은 tot+a이다. \(=초기화,  0+1 / 1+1 ... \)
* 11번 : 결과 도출 : 2 / 5 / 9 / 14 ~/ 54 / 65
* 12번 : 결과 도출 : 1 / 3 / 6 / 10 ~ / 45 / 55 \( 0+1 / 1+2 / 3+3 / ...  \) : 13번으로인 a값이 변한다.
* 13번 : a는 a+1을 대입한다.
* 14번 : 반복\(for\)문 닫기
* 15번 : 12번과 계산은 같지만, 반복문이 닫힌 메인 메소드안의 결과 이므로 최종 값만 도출된다. : 55

### 문자열과 숫자의 구분

```java
package book.ch2;

public class Quiz2 {

	public static void main(String[] args) {
		int a=1;
		int b=2;
		int c=3;
		int tot=a+b+c;
		System.out.println(a+"+"+b+"+"+c+"="+tot);//1+2+3=6
		System.out.println(a+"+"+b+"+"+c+"="+a+b+c);//1+2+3=123
		System.out.println(a+"+"+b+"+"+c+"="+(a+b+c));//1+2+3=6
		System.out.println("a+b*c===>"+(a+b*c));//a+b*c===>7
	    System.out.println("(a+b)*c===>"+((a+b)*c));//(a+b)*c===>9

	}

}

```

* 11번과 12번의 결과의 오른쪽 값에 주의하자. 11번의 결과는 문자열, 12번은 숫자로 구분한다.

```java
package book.ch2;

public class Quiz3 {

	public static void main(String[] args) {
		System.out.println(1+"2");//12
		System.out.println("1"+2);//12
		System.out.println(1+2);//3
		System.out.println("1"+"2");//12		
	}

}

```

* 문자열 + 문자열 = 문자열
* 문자열 + 숫자 = 문자열
* 숫자 + 숫자 = 숫자
* 숫자 + 문자열 = 문자열
* "1"은 숫자 1이 아닌 문자 1로 인식된다.

### 예시

```java
package book.ch2;

import javax.swing.JFrame;
//이 프로그램은 전자계산기를 구현한 것입니다.
public class MyCalc {
	public static void main(String[] args) {
		JFrame jf = new JFrame();//인스턴스화
		int width = 300;
		width = 600;
		int height = 400;
		jf.setSize(width, height);//width, height가 정수이기 때문에 setSize int를 사용
		String title = "전자 계산기";
		title = "카카오톡";
		jf.setTitle(title);
		boolean isOK = true;
	    jf.setVisible(isOK);//보이게한다=true 보이지않게한다=false
	}/////////////end of main
}////////////////end of MyCalc

```

* JFrame이라는 클래스를 jf로 인스턴스화하였고, 10번에서 width의 값은 600으로 초기화된다.
* 12번 : jf를 setSize\(600,400\)로 호출한다.
* 14번 : title은 카카오톡으로 초기화된다.
* 15번 : jf에 title로 카카오톡을 호출한다.

### 학습목표 이해

* [x] 변수를 선언할 수 있다.  - 변수의자료형 변수이름;
* [x] 클래스 안에서 변수와 메소드를 구분하여 말할 수 있다. - 메소드는 실행,명령하는 기능이고, 변수는 변화하는 값을 말한다.
* [x] 선언한 변수에 적당한 값으로 초기화를 할 수 있다. - 변수이름 = 값;
* [x] 초기화의 위치가 결과값에 어떠한 영향을 주는지 설명할 수 있다. - 초기화의 위치와 값에 따라 결과가 달라진다. Quiz1.class와 같이

후기 : 변수와 초기화 값을 정하는 방법을 많이 연구해보자. 선생님께서 변수를 사용하는 이유를 Variable1클래스 코드에서 찾아보라고 하셨을때, 일괄처리라고 답해 칭찬받았다.

