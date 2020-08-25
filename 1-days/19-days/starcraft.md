# StarCraft - 추상클래스, 인터페이스 설계

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

```java
package book.ch9;

public interface Movable {//인터페이스
	
	void move(int x, int y);//추상메서드
}
```

```java
package book.ch9;
//상속은 단일상속만 가능하다.
//인터페이스는 다중구현이 가능하다.
//단일 상속의 단점을 보완하기 위함.
public interface Attackable {//공격인터페이스
	void atack(Unit u);
}
```

```java
package book.ch9;
//인터페이스를 상속받는 인터페이스 = 다중구현
public interface Fightable extends Movable, Attackable{ }
```

```java
package book.ch9;

public class Figther extends Unit implements Fightable {

	@Override
	public void atack(Unit u) {
		// TODO Auto-generated method stub		
	}
	//오버라이드할때 접근 제한자는 반드시 더 넓은 범위로 처리해야한다.
	//메서드 이름은 소문자로 하자
	@Override
	public void move(int x, int y) {
		// TODO Auto-generated method stub		
	}
}
```

