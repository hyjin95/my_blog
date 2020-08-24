# Car.java



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
	//public int initDisplay() {//부모 메서드의 리턴타입, 파라미터를 변경하면 안되다.
	
	public int initDisplay(int i) {//overloading
		System.out.println("나는 프라이드 입니다.");
		return 0;
	}
	
	public void speedUp() {
		speed = speed + 1;//부모 Car에 있는 멤버 변수 speed를 그냥 가져다 쓸 수 있다. 
	}

}

```

```java
package book.ch8;

public class CarSimulation {

	public static void main(String[] args) {
		Car   myCar  = new Pride();//출력 : Car 디폴트 생성자 입니다. Pride 디폴트 생성자 입니다.
		Pride himCar = new Pride();//부모클래스를 먼저 호출, 로딩 한다 : Car 디폴트 생성자 입니다. Pride 디폴트 생성자 입니다.
		//Pride타입은 speedUp함수를 사용할 수 있다, Car타입은 speedUp함수를 호출할 수 없다.
		myCar.initDisplay();
		himCar.speedUp();
		//myCar.speedUp(); 자식한테 있는 함수이므로 오류발생
		
		//Avante와 Pride는 부모클래스가 같을뿐 서로 관계가 없는 남남인 클래스들이다, 아레와같이 인스턴스화 불가능하다.
		//Avante herCar = new Pride();
		
		//myCar = himCar;// myCar타입이 Car로 himCar타입 Pride 보다 크다. 성립한다. 
		//himCar = myCar; // myCar는 Car타입이지만 Pride클래스를 받는다. 타입이 himCar가 작으므로 성립하지않는다.
		himCar = (Pride)myCar;//캐스팅 연산자 ( )변수, Pride타입으로 myCar인스턴스변수를 강제 형전환한다.
		himCar.speedUp();//0 -> 1
		System.out.println("himCar.speed" +myCar.speed);//1 출력
		System.out.println("myCar.speed" +himCar.speed);//1 출력
	}
}
//6번 : Car타입 mycar는 Pride풍선을 들고있다. -> 하지만 Car타입이므로 Pride의 메서드인 speedUp은 호출할 수 없다.(Pride풍선안에 Car풍선이 candidate된 느낌)
//7번 : Pride 타입 himcar는 Pride풍선을 들고있다.
//18번 : myCar의 타입이 Pride로 강제전환되어서 speedUp메서드를 호출할 수 있다. myCar는 Pride타입이면서 Pride풍선을 들고있다. 그 풍선을 himCar주소번지와 공유한다.
//19번 : 

```

