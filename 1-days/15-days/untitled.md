# Untitled

### FlyBehavior.java

인터페이

```java
package duck;
/*
 * 인터페이스는 메서드 선언만 할 수 있다. =구현할 수 있는 바디가 없다.(반복문for if 등 , 출력문print 등 불가능)
 * 밑의 세 메서드들은 클래스가 정해지지 않았으므로 결정할 수 없다.
 * FlayBehavior의 구현체 클래스 = FlyWithWings, FlyNoWay
 */
public interface FlyBehavior {
	//public void fly() {}//{}==구현을 한다, 바디가 있다. 인터페이스에서는 오류
	public void fly();//바디가 없다 = 아직 결정할 수 없다.
	public int methodA();//리턴타입과 파라미터만 존재한다.
	public int methodA(int i);

}

```

### FlyWithWings.java

구현체 클래

```java
package duck;

//인터페이스로 FlyBehavior을 받아오면 꼭 갖고있어야하는 메서드가 정해진다.=필수메서드
//인터페이스의 메서드를 구현해주는 구현체 클래스2
public class FlyWithWings implements FlyBehavior {

	@Override
	public void fly() {
		System.out.println("나는 날고 있어요.");		
	}

	@Override
	public int methodA() {
		// TODO Auto-generated method stub
		return 0;
	}

	@Override
	public int methodA(int i) {
		// TODO Auto-generated method stub
		return 0;
	}
}
```

### FlyNoWay.java

구현체 클래스2

```java
package duck;

//구현체 클래스2
public class FlyNoWay implements FlyBehavior {

	@Override
	public void fly() {
		System.out.println("나는 날지 못합니다.");		
	}

	@Override
	public int methodA() {
		// TODO Auto-generated method stub
		return 0;
	}

	@Override
	public int methodA(int i) {
		// TODO Auto-generated method stub
		return 0;
	}
}
```

### MallardDuck.java

Duck 추상클래스를 상속하면서 구현체 클래스를 이용하는 클래스

```java
package duck;

/*
 * FlyBehavior는 인터페이스이다.
 * 메서드 선언만 되어있고 구현할수 있는 바디가 없는 상태 이므로 
 * 반드시 구현체 클래스가 있어야 활용할 수 있다.
 * 구현체 클래스가 implements를 갖는다.
 * 이 세상에 날수 있는 동물, 사물들이 많이 있다.
 * 사물이 갖는 공통분모를 찾기위해 인터페이스를 설계한다.
 * 단독으로는 인스턴스화 할 수 없다.->implements이용
 * 추상메서드를 갖고 있기때문에 결정되어있지않아서 쓸수 없기때문이다.
 */
public class MallardDuck extends Duck {//Duck추상클래스를 아빠로 갖는다.//implements FlyBehavior {//청둥오리는 인터페이스가 필요없다. 구현체 클래스를 가져올거니까.
	
	public MallardDuck() {
		//fb = new FlyBehavior();//호출 불가하다. 기능이 있는 클래스가 아니므로
		fb = new FlyWithWings();//구현체 클래스를 호출
		fb.fly();
		fb = new FlyNoWay();
		fb.fly();
	}

	@Override
	public void swimming() {
		System.out.println("나는 물위에 뜹니다.");		
	}
}
```

### WoodDuck.java

Duck 추상클래스를 상속하면서 구현체 클래스를 이용하는 클래스

```java
package duck;

public class WoodDuck extends Duck {
	//FlyBehavior fb = null;
	
	public WoodDuck() {
		fb = new FlyNoWay();
		fb.fly();
	}

	@Override
	public void swimming() {
		System.out.println("나는 물에 가라앉습니다.");		
	}
}
```

### RubberDuck.java

Duck 추상클래스를 상속하면서 구현체 클래스를 이용하는 클래스

```java
package duck;

public class RubberDuck extends Duck{
	//FlyBehavior fb = null;
	
	public RubberDuck() {
		fb = new FlyNoWay();
		fb.fly();
	}

	@Override
	public void swimming() {
	
	}
}
```

### Duck.java

추상클래스 : 일반 메서드 2개, 추상메서드 1

```java
package duck;

/*
*추상클래스 : 단독으로는 인스턴스화 할 수 없다.
*반드시 구현메서드가 있어야한다.
*그러나 일반 메서드도 가질 수 있다.
*그래서 인터페이스가 더 추상적이다.
*/
public abstract class Duck {//abstract = 추상클래스
	//추상클래스도 전변을 가질 수 있다.
	FlyBehavior fb = null;
	
	public void methodA() {
		fb.fly();
	}
	//{ }가 있는, 바디가 있는 메서드가 일반 메서드이다.
	public void display() {
		System.out.println("나는 오리 입니다.");
	}
	
	//swimming 은 추상메서드이다
	//인터페이스 에서는 abstract를 생략할 수 있지만(모두 다 추상메소드만 오므로)
	//그런데 추상클래스는 일반 메서드도 같이 공존하므로 구분할 수 있어야한다.
	//JVM이 인식하는 추상메서드는 abstract를 붙여야한다.
	public abstract void swimming();

}
```

### DuckSimulation.java

호

```java
package duck;

public class DuckSimulation {
//선언부와 생성부의 이름이 다르면 다형성(폴리모피즘)을 기대 할 수 있다.
//다형성이란 같은 이름의 메서드를 호출했는데 결과가 서로 다른것을 말한다.
//다형성의 전제조건은 선언부, 생성부 타입이 무조건 다를때 기대 할 수 있다.
//재사용성을 높이는 코딩방법으로 중요하다.
	
	public static void main(String[] args) {
		//추상클래스도 구현체 클래스가 있어야 만 한다.
		Duck myDuck = new MallardDuck();
		myDuck.methodA();//FlyBehavior.fly()
		WoodDuck herDuck = new WoodDuck();
		herDuck.methodA();
		herDuck.swimming();
		RubberDuck himDuck = new RubberDuck();
		himDuck.methodA();
		himDuck.swimming();
		//myDuck.fly();
	}
}

```

