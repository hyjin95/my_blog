# ArrayListTest.java

### List 인터페이스와 인터페이스 비교 연산자

```java
package book.ch8;

import java.util.ArrayList;
import java.util.List;

public class ArrayListTest {

	public static void main(String[] args) {
		//Vector는 멀티스레드에서 안전하다, 속도가 느리다. 스레드 경쟁이 일어난다. 같이쓴다, 공용화장실 = wait
		//list  는 싱글스레드에서 안전하다, 속도가 빠르다. 스레드 경쟁이 없다. 내 차의 네비게이션
		//List인터페이스는 util에 있는 것을 사용한다.
		List list = new ArrayList();
		Dog d = new Dog();
		list.add(d);
		Cat c = new Cat();
		list.add(c);
		for(int i=0;i<list.size();i++) {
			Object obj = list.get(i);
			if(obj instanceof Dog) {//타입을 비교하는 instanceof연산자
				System.out.println("개");
			}
			if(obj instanceof Cat) {
				System.out.println("김애용");
			}
			System.out.println(obj);
		}//end of for
	}//end of main
}
```

