---
description: 2020.08.20 - 16일차
---

# 16 Days -

필기 : private변수, public메서드, 재사용성\(함수의장점\), 지변의 유지  
FunctionExam1.java : 메서드 오버로딩, 생성자, 전변, 

```java
package function_first;

import book.ch2.Sonata;

public class FunctionExam1 {
	//선언부	
	//생성자 : 리턴타입이 없다. 생성자와 메서드의 차이점
	public FunctionExam1() { }
	
	void methodA() {//lev1
		//이 안에서 결정된 값을 외부에서는 사용할수 없다. 리턴타입이 void라 반환값이 없기때문.
		//다른 메서드에서도 사용하고싶다면 전역변수에 담아준다.
		//메서드는 스택에 담기므로 사라진다. = 유지불가
	}	
	int methodB() {//lev2 리턴타입이 int인 메서드	
		//다른 함수에서나 클래스(인스턴스화)에서 호출된 결과를 재사용할 수 있다.
		return 10;//이 함수의 반환값을 재사용할 수 있다.
	}	
	Sonata methodC() {//lev3 리턴타입이 클래스, Sonata가 public이라면 다른패키지의 것이지만 import하면 접근가능하다.
		return new Sonata();
	}	
	//메서드 오버로딩 : 무조건 파라미터의 갯수가 다르거나 타입이 달라야한다.
	private Sonata methodC(int i) {
		Sonata hercar = new Sonata();
		return hercar;
	}	
	//메인 메서드 : 가능하면 메인메서드는 슬림하게
	public static void main(String[] args) {
		FunctionExam1 fe = new FunctionExam1();
		int i = fe.methodB();//i=10
		
		Sonata mycar = fe.methodC(i);//Sonata에 i가 0으로 전역변수로 선언,초기화 되어있다. = 0 이 출력될 것이다.
		mycar.speed = mycar.speed + 100;//speed=100
		System.out.println(mycar.speed);//speed는 패키지가 달라서 접근할수 없다. Sonata클래스에가서 전변speed에 public을 추가해준다면 사용할 수 있다.
		//메서드 C에서 생성된 소타나의 속도를 50으로 변경해주세요.
		//methodc()Sonata한대, methodC(int i)Sonata한대, = 서로다른 자동차
		Sonata himcar = fe.methodC();//주인없는 메서드
		himcar.speed = 50;
		System.out.println(himcar.speed);
	}
}
```

```java
package book.ch6;
/*
 * methodA에 선언된 지변을 외부 다른 메서드에서 유지 또는 사용할 수 있나요?
 * 1)static으로 선언된 변수를 사용하는것.(전역변수이용)
 * 2)전변에서 초기화하기. this.i = i;(전역변수이용)
 * 사용불가합니다. static이나 전역변수만 가능해요
 */
public class LocalVariable {
	int i = 1;
	//static int i = 2;//i가중복선언 되어있기때문에 이름을 다르게 해야한다.
	static int j = 2;//=싱글톤이다.
	
	void methodA() {
		int i;
		i=10;
		System.out.println(i);//10
		//static타입의 변수를 non-static영역에서 사용하는건 가능하다.
		System.out.println(j);//2
		
		this.i = i;//지역변수 i=10을 전역변수에 대입한다.
	}	
	//static영역에서 non-static영역은 사용불가능하다.
	//인스턴스화하면 사용가능하다.
	public static void main(String[] args) {
		//메인메서드에서 지역변수 i는 접근이 불가하다.
		LocalVariable lv = new LocalVariable();
		System.out.println(lv.i);//인스턴스변수로 접근할수 있는 변수는 전역변수 뿐이다. = 1출력
		System.out.println(j);//2출력
		j = 50;
		lv.methodA();//j=2->50
		System.out.println(lv.i);//위 methodA에서 전역변수가 10으로 변했으므로 10출력
	}
}
```

### Company.java-private static

private : 해당 클래스에서만, static : 싱글톤, 단 하나의 원

```java
package book.ch6;

public class Company {
	
	private static Company instance = null;//new Company();
	
	private Company() {
		System.out.println("Company 디폴트 생성자 호출성공");
	}
	
	public static Company getInstance() {
		if(instance == null) {//인스턴스선언만 되어잇니?
			instance = new Company();//그럼 생성
		}			
		return instance;
	}
}
//private는 데이터를 get으로 걸러 받을때, 조건을 걸어야할때 사용
```

```java
package book.ch6;

public class CompanyTest {

	public static void main(String[] args) {
		//Company cp = new Company(); //생성자가 private이므로 호출불가
		Company cp1 = Company.getInstance();//static이므로 : 타입.메서드();
		Company cp2 = Company.getInstance();//7번에서 인스턴스가 생성되었으므로 null이 아니다. 생성자를 호출하지 않는다.
		System.out.println(cp1 == cp2);//싱글톤이므로 주소가 두개지만 하나를 바라보고있다. true이다.
	}
}
```

