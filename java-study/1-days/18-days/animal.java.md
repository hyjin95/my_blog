# Animal.java - 추상클래스

### Animal.java - 추상클래스

```java
package book.ch8;

public abstract class Animal { }
```

### Dog.java - 구현체 클래스

```java
package book.ch8;

public class Dog extends Animal { }
```

### Cat.java - 구현체 클래스2

```java
package book.ch8;

public class Cat extends Animal { }
```

### ObjectTest.java

```java
package book.ch8;
//모든 클래스의 부모 Object
public class ObjectTest {

	public static void main(String[] args) {
		Dog d  = new Dog();//실체가 생성된 d주소 풍선
		Dog d2 = null;//실체가 생기지않은 d2주소 풍선
		d2 = d;//d2 주소의 풍선이 d를 바라본다. = 같은주소, 같은 풍선 
		Cat c  = new Cat();

		if(d.equals(c)) {//d랑 c랑 같으면 true 출력
			System.out.println("ture");			
		}//end of if
		if(!d.equals(c)) {//d랑 c랑 다르면 false 출력
			System.out.println("false");			
		}//end of if
		System.out.println(d2==d);//true출력
		System.out.println(d2.equals(d));//true출력
	}
}
```

* 12번에서 Dog 클래스에 메서드가 없는 상태 임에도 d.equals( )가 성립되는 이유는 부모의 부모인 Object클래스의 것이기 때문이다.
* 6번 : Dog타입 인스턴스변수 d는 Dog풍선을 생성한다, 갖는다.
* 7번 : Dog타입 인스턴스변수 d2는 Candidate상태의 풍선과 주소를 갖는다.
* 8번 : d2에 d가 대입되면서 d2와 d는 이름만 다른, 같은 주소번지와 같은 풍선을 갖는다.
* 11번 : d와 c 객체가 같으면
* 14번 : d와 c 객체가 다르면
* 17번 : d2와 d는 서로 같은 주소번지 값을 가리킨다.

### Object클래스의 메서드 호출

```java
package book.ch8;
//모든 클래스의 부모 Object
public class ObjectTest {

	public static void main(String[] args) {
		Dog d  = new Dog();//실체가 생성된 d주소 풍선

		System.out.println(d.getClass());//어떤 클래스의 인스턴스인지를 알 수 있도록 객체의 클래스를 반환한다.
		System.out.println(d.hashCode());//고유 ID를 말한다. 그 객체가 가지는 고유한 ID
		System.out.println(d.toString());//UI 솔루션을 자바코드로 말아서 사용할 때 사용한다.
	}
}
```

* 8번 : **d.getClass( )**  : 어떤 클래스의 인스턴스인지를 알 수 있도록 객체의 클래스를 반환한다.
* 9번 : **d.hashCode( )**  : 고유 ID를 말한다. 그 객체가 가지는 고유한 ID
* 10번 : **d.toString( )**  : UI 솔루션을 자바코드로 말아서 사용할 때 사용한다.
