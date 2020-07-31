---
description: 2020.07.31 - 3일차
---

# 3 Days - 변수의 종류와 위치

### 자료형의 종류

1. Primative Data Type : 기본형  정수형 : byte, short, int, long 문자형 : char 실수형 : float, double 논리형 : boolean
2. Reference Type : 참조형

### 변수를 이용해 코드 만들어보기

#### 내가 한 것

```java
package book.ch2;

public class Sonata {
	int speed = 0; //speed 초기화
	void speedUp() {
		int speed = speed + 1; 		
	}
	void speedDown( ) {
		int speed = speed - 1;		
	} 
	//메인 메소드 = 메인 스레드(Thread) - 자바에서 제공되는 클래스, 우선순위 1번
	public static void main(String[] args) {
		Sonata mycar = new Sonata();
		Sonata hercar = new Sonata();
		mycar.speedUp();
	    hercar.speedDown();
	    
	    System.out.println(mycar.speedUp);
	    System.out.println(hercar.speedDown);
	}

}

```

#### 선생님이 하신 것

```java
package book.ch2;

public class Sonata_Teacher {
	int speed = 0; 
	void speedUp() {
		speed = (speed + 1); 
	}
	void speedDown( ) {
		speed = (speed - 1);
	} 
	//메인 스레드(Thread)-우선순위1번
	public static void main(String[] args) {
		Sonata_Teacher mycar = new Sonata_Teacher();
		mycar.speedUp();
		Sonata_Teacher hercar = new Sonata_Teacher();
		hercar.speedUp();
		
		System.out.println(hercar.speed); //hercar의 현재 속도 (기본값=0, Up상태 호출, 결과=1)
	    System.out.println();
	}//end of main
}

```

### 전역변수와 지역변수

```java
package book.ch2;

public class P49 {
	//전역변수는 클래스 전역에서 사용할 수 있다.
	int i; //전역변수-heap[global variable, member variable, instance variable]
	//i=2;	
	public static void main(String[] args) {
		P49 p49 = new P49();
		p49.methodA();
		int i = 5; //지역변수-stack[local variable, automatic variable]
		i = 10;
		p49.methodB(i);
		float f = 3.14f;
		double d = 3.1456;
		boolean isOK;
		isOK = false;
		System.out.println(isOK);
		
	}
	void methodA() {
			System.out.println("전역변수 i==>"+i); //0
		//질문 : methodA에서 10번에서 선언한 변수 1값 10을 출력하고싶으면?
	}
	void methodB(int i) {//파라미터에 선언한 변수도 지변이다.
		System.out.println("지역변수 i는"+i);//10
	}

}

```

### 대입, 증가, 감소연산자

```java
package book.ch3;

public class P77_1 {

	public static void main(String[] args) {
		int x = 1;
		int y = 2;
		if((++x<y)&(x>y--)) {//참일때
			System.out.println("참일때");			
		}
		else {//거짓말일때
			System.out.println("거짓말일때");			
		}
		//거짓말일때, x=2, y=1
		System.out.println("x="+x+", y="+y);		
		System.out.println("=======[[after]]=======");		
		//위의 결과가 밑에 조건문에 영향을 끼친다. 똑같은 조건으로 하려면 다시 초기화를 해야한다.
		x = 1;
		y = 2;
		if((++x<y)&&(x>y--)) {//참일때
			System.out.println("참일때");					
		}
		else {//거짓말일때
			System.out.println("거짓말일때");			
		}
		//초기화를 안했을 때, x=3, y=1, 첫 조건이 False라 두번째 조건은 진행하지 않는다.
		//거짓말일때, x=2, y=2
		System.out.println("x="+x+", y="+y);	
		
		x = 1;
		y = 2;
		if((++x<=y)|(x>y--)) {//참일때
			System.out.println("참일때");					
		}
		else {//거짓말일때
			System.out.println("거짓말일때");			
		}		
		//참일때, x=2, y=1
		System.out.println("x="+x+", y="+y);		 
		
		x = 1;
		y = 2;
		if((++x<=y)||(x>y--)) {//참일때
			System.out.println("참일때");					
		}
		else {//거짓말일때
			System.out.println("거짓말일때");			
		}		
		//참일때, x=2, y=2
		System.out.println("x="+x+", y="+y);	
	}

}
```

### 리턴 타입과 값

```java
package book.ch3;

public class Parameter1 {
	//Parameter1 p2 = new Parameter1();//p2는 전변이다.
	//값에 의한 호출입니다.
	//그래서 지변 x에는 10이 담기고, 지변y 에는 20이 담긴다.
	void methodA(int x, int y) {//x=10. y=20
		System.out.println("methodA호출 성공");
		//p2.methodA(1,2);
		System.out.println(x+y);
		
	}
	//메소드 선언할때 반환 타입을 결정할 수 있다.
	//리턴 타입이 있는 경우 실행문 마지막에 반드시 return이라는 예약어를
	//써 주어야 한다.
	//이때, 리턴 다음에는 값이나 변수명이 올 수 있다.
	//단, 변수의 타입이 리턴의 타입과 반드시 일치해야 한다.
	double methodB(double d1, double d2) {
		System.out.print(d1+d2);
		return 0;
	}
	
	public static void main(String[] args) {
	//RAM영역에 Prameter1클래스를 로딩하기
		Parameter1 p1 = new Parameter1();//p1은 지변이다.
		p1.methodA(10, 20);
		double hap = p1.methodA(10, 20);
		double hap2 = p1.methodB(10, 20);

	}

}

```

```java
package book.ch3;

public class Print {

	public static void main(String[] args) {
		//print메소드는 자바에서 제공되는 메소드이므로 반드시 파라미터의 갯수나
		//타입이 반드시 일치해야 문법에러를 피할 수 있다.
		//System.out.print(); //파라미터의 부재
		System.out.println();//줄바꿈의 역할이 있다.
		System.out.print(1);
		System.out.println();
		System.out.println("하나");
		Parameter1 p1 = new Parameter1();
		System.out.print("\n"+p1.methodB(1.5,  2.5));

	}

}

```

