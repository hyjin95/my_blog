---
description: 2020.07.31 - 3일차
---

# 3 Days - 변수, 연산자와 리턴값

### 자료형의 종류

1. Primative Data Type : 기본형  정수형 : byte, short, int, long 문자형 : char 실수형 : float, double 논리형 : boolean
2. Reference Type : 참조형

### 지역변수, 전역변

* RAM은 stack과 heap으로 구분할 수 있다.  stack : 공간이 더 작고, 인스턴스변수와 지변수가 저장된다. heap : 공간이 더 크며, 클래스와 클래스 안의 변수가 저장된다.
* 로컬변수 :  메소드안에서 선언된 변수와 파라미터 변를 말하며, 메소드 범위인 "{-}" 중괄호 안에서만 유지된다. 메소드 밖에서는 값이 유지 되지 않고, 반드시 초기화를 해야한다.
* 멤변수 : 클래스 영역 변수를 말하며, 인스턴스를 가지고 있는 동안에는 초기화 하지 않아 기본값으로 유지된다. 
* 인스턴스 변수 : 클래스를 인스턴스화 할때 필요한 주소 개념으로 stack에 저장된다.
* 같은 변수를 두 번 선언할 수 있다. \(지역변수, 전역변수\) 
* 참고 링크 : [https://itellyhood.tistory.com/m/32](https://itellyhood.tistory.com/m/32)

### i에 대하

* 타입 int 의 경우에 RAM에 저장되는 것들은 각각의 숫자 주소를 갖게 된다.  이를 불러올때, i를 사용하는데 이 i 지역변수로만 사용한다.

### 변수를 이용해 코드 만들기

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
	    
	  System.out.println(mycar.speedUp);//1
	  System.out.println(hercar.speedDown);//-1
	}

}

```

#### 선생님

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
		
		//hercar의 현재 속도 (기본값=0, Up상태 호출, 결과=1)
		System.out.println(hercar.speed); 
	  System.out.println(mycar.speed);
	}//end of main
}

```

* 메인메소드를 메인스레드\(Main Thread\)라고도 하며, 실행 우선순위 1번이다.
* 수식을 사용할 때 괄호는 써도되고 안써도 되지만, 계산에 영향을 주는 때에는 주의해야한다.
* speed를 클래스에서 int로 선언 했으므로, speedUP과 speedDown값을 초기화 할때에는 타입을 쓰지 않아도 된다.
* 메인메소드에서 불러온 메소드 안에 speed가 수식으로 지정되어있기때문에 출력할때 speed만 출력하여도 값이 나다. \(현재속도\)

### 전역변수와 지역변수

```java
package book.ch2;

public class P49 {
	int i; 
		
	public static void main(String[] args) {
		P49 p49 = new P49();
		p49.methodA();
		int i = 5; 
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
	void methodB(int i) {
		System.out.println("지역변수 i는"+i);//10
	}

}

```

* 클래스멤버변로 i가 int타입으로 선언만 되었기 때문에 클래스에서 i는 기본 값인 0이 된다.
* main thread안의 i와 methodB의 파라미터 i는 지역변수이다.
*  p49는 인스턴스 변이다.
* methodA에서의 i 값은 선언, 초기화 되지않았기 때문에 클래스안의 인스턴변수 i값을 갖게된다. 이 i는 main Thread안에 호출되어도 값에 영향을 받지 않는다.
* methodB는 파라미터에 int i 가 main Thread에 호출되면 초기화된 i 값에 영향을 받는다. 
* float 타입을 사용할때는 값뒤에 f를 붙여야한다.

### 대입, 증감 연산자, &과 &&, \|와 \|\|, if와 else

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

* 대입연산자 : = 같다 : ==
* x = ++y;  /  x = --y;  :  증감연산자가 앞에 있으면 선 증감, 후 대입으로 이루어진다.
* x = y++;  /  x = y--;  :  증감연산자가 뒤에 있으면 선 대입, 후 증감으로 이루어진다.
* &와 \|는 조건 모두를 계산하기때문에 x와 y의 값이 계산이 모두 이루어진 뒤에 정해진다.
* &&와 \|\|는 첫 조건이 false면 더이상 계산하지 않기 때문에 x와 y의 값이 진행되지 않는다.
* if : \(~가 참이면\) {-} else\(거짓이면\){-}

### 리턴 타입과 값

```java
package book.ch3;

public class Parameter1 {
	//Parameter1 p2 = new Parameter1();//p2는 인스턴스변이다.
	//값에 의한 호출로 지변 x에는 10이, 지변 y에는 20이 담긴다.
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
		Parameter1 p1 = new Parameter1();//p1은 인스턴스변이다.
		p1.methodA(10, 20);
		double hap = p1.methodA(10, 20);
		double hap2 = p1.methodB(10, 20);

	}

}

```

* 리턴 타입 : 메소드를 선언할때 반환 타입을 정할 수 있는데 이를 리턴타입이라 한다. 실행문 마지막에 반드시 return이라는 예약어를 써주어야 하며, return다음에 값이나 변수명이 올 수 있다. 이때, 변수의 타입이 리턴의 타입과 반드시 일치해야 한다.

### System.out.print\(\)

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
		System.out.print("\n"+p1.methodB(1.5,  2.5)); //4.0

	}

}

```

* print는 자바에서 제공되므로 반드시 파라미터의 갯수와 타입이 일치해야 문법에러를 피할 수 있다.
* print\(\)처럼 파라미터가 비어있으면 출력되지 않는다.
* println\(\)은 줄바꿈이 포함되어있어 출력된다.
* 14번 처럼 인스턴스변수p1을 이용하여 다른 클래스의 메서드를 호출할 수 있다. 이 경우에는 print의 파라미터의 갯수와 타입이 Parameter1클래스 methodB 파라미터의 갯수와 타입이 일치하기때문에 출력 가능하다.

후기 : 자료를 많이 찾아봐서 끝까지 이해하도록 노력하

