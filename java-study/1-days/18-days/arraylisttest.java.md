# ArrayListTest.java - 인터페이스비교연산자

### List 인터페이스와 인터페이스 비교 연산자

```java
package book.ch8;

import java.util.ArrayList;
import java.util.List;

public class ArrayListTest {

	public static void main(String[] args) {
		
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

* List인터페이스는 add할때 순서대로 담는다.
* 15번 : list.size = 방의 갯수
* 17번 : 입력된 obj의 타입의 Dog와 같다면
* 20번 : 입력된 obj의 타입이 Cat과 같다면

