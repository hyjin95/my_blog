# Car.java - 상속관계 이해

### Car.java - 추상클래스

```java
package book.ch8;

public abstract class Car {//추상클래스
	//기본 멤버변수
	int speed;
	int wheelNum;
	
	public Car() {//생성자를 가질 수 있다.
		System.out.println("Car 디폴트 생성자 입니다.");
	}
	
	public abstract void initDisplay();//추상메서드
	
	//모든자동차는 정지기능이 있다.
	public void stop() {//일반 메서드
		if(speed>0) speed = 0;
	}
}
```

* 추상클래스이므로 클래스와 추상메서드에 abstract가 붙는다.
* 변수, 생성자, 일반메서드, 추상메서드를 모두 갖는다.
* 추상메서드가 있으므로 단독 인스턴스화는 할 수 없다.

### Pride.java - Car:부모, Pride:자식

```java
package book.ch8;

public class Pride extends Car {
	
	public Pride() {
		System.out.println("Pride 디폴트 생성자 입니다.");
	}

	@Override
	public void initDisplay() {
		System.out.println("나는 프라이드 입니다.");		
	}
	
	//부모가 가진 메서드 인지 원형을 어떻게 아는걸까?
	//서로 상속관계에 있더라도 overloading이 가능하다.
	//@Override
	//public int initDisplay() {
	
	public int initDisplay(int i) {//overloading
		System.out.println("나는 프라이드 입니다.");
		return 0;
	}
	
	public void speedUp() {
		speed = speed + 1; 
	}
}
```

* Pride클래스는 일반 클래스이지만 Car추상클래스를 상속받는다. -&gt; Car의 구현메서드가 존재해야한다. = overriding\(부모메서드와 같은 리턴타입, 같은 파라미터\)
* 19번은 구현메서드가 아닌 다른 메서드이다. = overloading, 파라미터가 다르다.
* 25번 : 자식클래스 Pride는 부모클래스 Car의 멤버변수를 가져다 쓸 수 있다.

### CarSimulation.java - 실행문

```java
package book.ch8;

public class CarSimulation {

	public static void main(String[] args) {
		Car   myCar  = new Pride();
		Pride himCar = new Pride();
		//Pride타입은 speedUp함수를 사용할 수 있다, Car타입은 speedUp함수를 호출할 수 없다.
		myCar.initDisplay();
		himCar.speedUp();
		//myCar.speedUp(); 자식한테 있는 함수이므로 오류발생
		
		//Avante와 Pride는 부모클래스가 같을뿐 서로 관계가 없는 남남인 클래스들이다, 아레와같이 인스턴스화 불가능하다.
		//Avante herCar = new Pride();
		
		//myCar = himCar; 
		//himCar = myCar; //myCar는 Car타입이지만 Pride클래스를 받는다.
		himCar = (Pride)myCar;//캐스팅 연산자 ( )변수, myCar인스턴스변수를 강제 형전환
		himCar.speedUp();//0 -> 1
		System.out.println("himCar.speed" +myCar.speed);//1 출력
		System.out.println("myCar.speed" +himCar.speed);//1 출력
	}
}
//6번 : Car타입 mycar는 Pride풍선을 들고있다. -> 하지만 Car타입이므로 Pride의 메서드인 speedUp은 호출할 수 없다.(Pride풍선안에 Car풍선이 candidate된 느낌)
//7번 : Pride 타입 himcar는 Pride풍선을 들고있다.
//18번 : myCar의 타입이 Pride로 강제전환되어서 speedUp메서드를 호출할 수 있다. myCar는 Pride타입이면서 Pride풍선을 들고있다. 그 풍선을 himCar주소번지와 공유한다.

```

* 상속관계에서 인스턴스화가 일어나면 부모클래스의 생성자를 먼저 호출, 로딩한다.
* 6번 : 인스턴스변수 myCar는 Car타입이지만 Pride풍선을 갖는다. = Pride의 함수를 호출할 수 없다.
* 16번 : mycar의 타입이 Car로, himCar의 타입 Pride보다 크므로 성립한다. 
* 17번 : 성립하지 않는다.
* 18번 : myCar를 캐스팅연산자를 이용해 타입을 Pride로 강제 형전환시킨다. - himCar에 이를 대입해 둘다 같은 Pride를 바라보게된다. - myCar는 Pride타입으로, Pride의 함수를 호출 할 수 있다.
* 출력결과 - 6번 : Car 디폴트 생성자 입니다. Pride디폴트 생성자 입니다. - 7번 : Car 디폴트 생성자 입니다. Pride 디폴트 생성자 입니다. - 20번 : himCar.speed 1 - 21번 : myCar.speed 1
* 18번이 없다면 20번은 0, 21번은 1이 출력된다.

