# FunctionExam.java

```java
package function_first;

import book.ch2.Sonata;

public class FunctionExam1 {
	//선언부	
	//생성자 : 리턴타입이 없다. 생성자와 메서드의 차이점
	public FunctionExam1() { }
	
	void methodA() {//lev1	
	}	
	int methodB() {//lev2 리턴타입 int
		return 10;//이 함수의 반환값을 재사용할 수 있다.
	}	
	Sonata methodC() {//lev3 리턴타입 클래스
		return new Sonata();
	}	
	//메서드 오버로딩 : 무조건 파라미터의 갯수가 다르거나 타입이 달라야한다.
	private Sonata methodC(int i) {
		Sonata hercar = new Sonata();
		return hercar;
	}	
	//메인 메서드 : 가능하면 메인메서드는 슬림하게
	public static void main(String[] args) {
		FunctionExam1 fe = new FunctionExam1();//인스턴스화 및 생성자호
		int i = fe.methodB();//i=10
		
		Sonata mycar = fe.methodC(i);//Sonata에 멤버변수로 i=0; 되어있다. = 0 출력
		mycar.speed = mycar.speed + 100;//speed=100
		System.out.println(mycar.speed);//speed는 패키지가 달라서 접근할수 없다. 
		//Sonata클래스에가서 전변speed에 public을 추가해준다면 사용할 수 있다.
		//메서드 C에서 생성된 소타나의 속도를 50으로 변경해주세요.
		//methodc()Sonata한대, methodC(int i)Sonata한대, = 서로다른 자동차
		Sonata himcar = fe.methodC();
		himcar.speed = 50;
		System.out.println(himcar.speed);//50
	}
}
```

* 10번 : methodA는 void타입\(반환X\)의 메서드이다. 메서드는 stack영역에 저장되며 { }안에 선언된 변수들은 지역변수로, 메서드 외부에서는 인스턴스화를 제외하고는 사용될수 없다.
* 12번 : methodB는 int타입메서드로 return도 int로 해야만한다. 다른 함수에서나 클래스에서 인스턴스화 되어 사용되어 나온 반환값을 재사용 할 수 있다.
* 15번 : Sonata클래스를 타입으로 갖는 methodC이다. Sonata는 다른 패키지에 있지만 public클래스로서 import하여 사용할 수 있다.
* 19번 : 접근제한자가 private 메서드는 다른 클래스에서는 사용될 수없다. int타입 값을 파라미터로 갖는다. Sonata클래스를 인스턴스화하여 인스턴스변수를 반환값으로 한다.
* 15번과 19번의 메서드는 overloading의 규칙을 따른다.
* 26번 : int i = 지역변수, fe클래스의 methodB의 반환값을 담는다.
* 다른 패키지에 있는 Sonata클래스의speed변수가 public으로 선언되어있으면 사용가능하다.



