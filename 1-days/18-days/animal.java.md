# Animal.java - 추상클래스

### Animal.java - 추상클래스

```java
package book.ch8;

public abstract class Animal { }//추상클래스
```

### Dog.java - 구현체 클래스

```java
package book.ch8;
//Animal클레스에 대한 구현체 클래스
public class Dog extends Animal { }
```

### Cat.java - 구현체 클래스2

```java
package book.ch8;
//Animal클래스에 대한 구현체 클래스 2
public class Cat extends Animal { }
```

### ObjectTest.java

```java
package book.ch8;
//모든 클래스의 부모 Object
public class ObjectTest {

	public static void main(String[] args) {
		Dog d  = new Dog();//실체가 생성된 d주소의 풍선
		Dog d2 = null;//실체가 생기지않은 candidata상태의 d2주소 풍선
		d2 = d;//실체가 없던 d2 주소의 풍선이 d를 바라본다. = 같은주소를 갖게 되었다, 같은 풍선 
		Cat c  = new Cat();
		//equals메서드로 비교 : 그 변수가 가리키는 객체가 같은것인지를 비교하는 것
		// == 로 비교            : 그 변수들의 주소번지 값이 같은지를 비교하는 것
		if(d.equals(c)) {//d랑 c랑 같으면 true 출력
			System.out.println("ture");			
		}//end of if
		if(!d.equals(c)) {//d랑 c랑 다르면 false 출력
			System.out.println("false");			
		}//end of if
		System.out.println(d2==d);//true출력 두개 변수는 서로같은 주소번지 값을 가리킨다.
		System.out.println(d2.equals(d));//true출력
		System.out.println(d.getClass());//어떤 클래스의 인스턴스인지를 알 수 있도록 객체의 클래스를 반환한다.
		System.out.println(d.hashCode());//고유 ID를 말한다. 그 객체가 가지는 고유한 ID
		System.out.println(d.toString());//UI 솔루션을 자바코드로 말아서 사용할 때 사용한다.
	}
}
```

