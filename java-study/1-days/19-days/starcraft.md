# StarCraft - 복습

### Unit.java - 추상클래스

```java
package book.ch9;

public abstract class Unit {//추상클래스
	int x, y;
	
	public Unit() {	}
	
	public Unit(int x, int y) {//유닛이 죽은 자리를 기억하는 생성자
		this.x = x;
		this.y = y;
	}
	
	abstract void move(int x, int y);//추상메서드
	
	void stop() {
		System.out.println("현재 위치에 정지");
	}
}
```

### Marine.java - 구상클래스

```java
package book.ch9;

public class Marine extends Unit {//추상클래스를 상속받는 구상클래스

	@Override
	void move(int x, int y) {
		// TODO Auto-generated method stub		
	}
	
	void stimPack() { }//공격메서드
}
```

* Unit의 추상메서드인 move를 반드시 Override해야한다.

### Tank.java - 구상클래스2

```java
package book.ch9;

public class Tank extends Unit {//추상클래스를 상속받는 구상클래스

	@Override
	void move(int x, int y) {
		// TODO Auto-generated method stub		
	}
	
	void chaneMode() {}//이동하면서 공격하는 모드
}
```

* Unit의 추상메서드인 move를 반드시 Override해야한다.

### Dropship.java - 구상클래스3

```java
package book.ch9;

public class Dropship extends Unit {//추상클래스를 상속받는 구상클래스

	@Override
	void move(int x, int y) {
		// TODO Auto-generated method stub		
	}
	//선택한 유닛을 태운다.
	void load() {}
	
	//대상 유닛을 내린다.
	void unload() {}
}
```

* Unit의 추상메서드인 move를 반드시 Override해야한다.

### PlayerTest.java 

```java
package book.ch9;

public class PlayerTest {

	public static void main(String[] args) {
		Unit[] group = new Unit[4];
		group[0] = new Marine();
		group[1] = new Tank();
		group[2] = new Marine();
		group[3] = new Dropship();
		
		for(int i=0; i<group.length;i++) {//그룹을 원하는 좌표에 이동
			group[i].move(100, 300);
		}
		//개선된 for문은 전체를 모두 출력할때 사용한다.
		for(Object obj:group) {
			if(obj instanceof Marine) {
				System.out.println("마린이 100,300으로 이동했다.");
			}
			if(obj instanceof Tank) {
				System.out.println("탱크가 100,300으로 이동했다.");
			}
			if(obj instanceof Dropship) {
				System.out.println("드랍쉽이 100,300으로 이동했다.");
			}
		}
	}
}
```

* 구상클래스로 실체화된 유닛들을 움직여보는 클래스
* 출력결과 - 마린이 100,300으로 이동했다.  - 탱크가 100,300으로 이동했다.  - 마린이 100,300으로 이동했다.  - 드랍쉽이 100,300으로 이동했다.

## 다중구현

### Movable.java - 인터페이스

```java
package book.ch9;

public interface Movable {//인터페이스
	
	void move(int x, int y);//추상메서드
}
```

### Attackable.java - 인터페이스

```java
package book.ch9;

public interface Attackable {//공격인터페이스
	void atack(Unit u);
}
```

### Fightable.java -  인터페이스를 상속받는 인터페이스

```java
package book.ch9;

public interface Fightable extends Movable, Attackable{ }
```

* 인터페이스는 여러 인터페이스를 상속받을 수 있다. = 다중구현
* 인터페이스 안에서나, 다른 클래스에서도 인터페이스는 여러개를 상속받을 수 있다.

### Fighter.java - 추상클래스와 인터페이스의 Override

```java
package book.ch9;

public class Figther extends Unit implements Fightable {

	@Override
	public void atack(Unit u) {
		// TODO Auto-generated method stub		
	}
	
	//메서드 이름은 소문자로 하자
	@Override
	public void move(int x, int y) {
		// TODO Auto-generated method stub		
	}
}
```

* 상속받고있는 Unit추상클래스에도 추상메서드가 있고, Fightable인터페이스에도 추상메서드가 있다.
* 오버라이드 할때 접근제한자는 반드시 더 넓은 범위로 처리해야한다. - Unit, Movable은 같은 추상메서드를 갖는데 둘다 접근제한자가 없지만, Fighter클래스에서는 그보다 넓은 접근제한자인 public을 주어야만 오류가 생기지않는다.

