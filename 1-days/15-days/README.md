---
description: 2020.08.19 - 15일차
---

# 15 Days - 상속, 추상클래스, 인터페이스, 구현체클래스, 다형성

### InstanceTest.java

```java
package desigin.test;

import javax.swing.JButton;
import javax.swing.JFrame;

//extends =상속, this(아빠꺼를 쓸수잇는 함수)를 쓸 수 있다. 
//JFrame(아빠)을 상속받는 InstanceTest(자식)클래스이다.=아들 안에 아빠
//예외 : static안에서는 this를 사용할 수 없다. : static=하나를 공유, 나눠쓰는것
//ex)Integer.=클래스와 같이 static은 클래스안에 있지만 Heap내부에서 따로 존재한다.공간이 많지않다. 적게사용하자!
public class InstanceTest extends JFrame {
	//선언부의 타입과 생성부의 타입이 다를 수 있다. 
	JButton jbtn_north = null;//사용되는 메서드에서 초기화 하지않으면 NullPointEception이 일어난다.
	JButton jbtn_south = null;
	
	public InstanceTest() {
		jbtn_north = new JButton("북쪽(버튼)");
	}//end of InstanceTest
	
	public InstanceTest(String label) {
		jbtn_south = new JButton("label");		
	}
	
	public void initDisplay() {
		if(jbtn_north!=null)//south버튼이 add되어있지않으므로 비어있다.
			this.add("North", jbtn_north);
		this.setTitle("인스턴스화 분리");
		this.setSize(400, 500);//내거 아님, 아빠JFrame이 갖고있는 메서드
		this.setVisible(true);
	}//end of initDisplay

	public static void main(String[] args) {
		InstanceTest it = new InstanceTest("남쪽");
		it.initDisplay();
		//JFrame jf = new InstanceTest();//왼쪽항에 큰 타입, 오른쪽 항에 작은 타입이 와야하므로 성립하지 않는다.
		//jf.initDisplay(); initDisplay가 아들한테 있지 아빠한테 없기때문에 호출불가 오류
		//아빠타입 변수로는 아들타입에 선언된 메서드를 호출 할 수 없다.
	}//end of main
}//end of class
```

### Father.java , Son.java

```java
package desigin.test;

class Father{
	
	public void methodA() {
		System.out.println("Father ---> methodA 호출성공");
	}//end of methodA
}//end of class

class Son extends Father {
	@Override
	public void methodA() {
		System.out.println("Son ---> methodA 호출성공");
	}
}
public class FatherSmulation {
	
	public static void main(String[] args) {
		Father fa = new Father();
		fa.methodA();
		Son    so = new Son();
		so.methodA();
		//두 개의 클래스 사이가 부자관계에 있을때에만 사용 가능하다.
		//출력결과는 자녀에 정의된 내용이 동일하게 적용된다.
		//이때 Father가 정의하고 있는 메서드는 은닉메서드가 된다.
		Father fa2 = new Son();
		fa2.methodA();
	}//end of main
}
```

### Outter.java

```java
package desigin.test;

class Outter {
	int i = 10;
	public Outter() {
		Inner in = new Inner();
		in.go();
	}
	
	public void come() {
		System.out.println("come 호출 성공");
	}
	//내부 클래스 라고 한다.
	//반드시 외부 클래스를 인스턴스화 해야만 접근이 가능하다.
	//외부 클래스의 멤버 변수를 사용할수 있다.
	class Inner {//클래스 안에 클래스쓰기
		public void go() {
			System.out.println("go 호출 성공 : "+i);
		}
		
	}//end of Inner class
}//end of Outter class

public class OutterTest {

	public static void main(String[] args) {
		Outter ot = new Outter();
		//Inner in = ot.new Inner();
		//in.go();
		ot.come();
	}
}
```

