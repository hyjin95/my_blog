---
description: 2020.08.19 - 15일차
---

# 15 Days - 상속, 추상클래스, 인터페이스, 구현체클래스, 다형성

### 복습

1. 화면 - Conteiner : 가장 밑바닥 - JFrame : 속지, 단독으로 화면을 나타낼 수 있다. - JPanel : 속지 위에서 화면을 구성해주는 클래스
2. Jpanel 인스턴스화와 생성자 - Jpanel jp = new JPanel\(\); - JPanel의 DefaultLayout은 FlowLayout이다. - BorderLayout으로 set 하는 법 : 소유주 주소번지.setLayout\(new BorderLayout\(\)\);
3. FlowLayout, BorderLayout - Flow : 정렬레이아웃, 버튼이 2개 있는 창을 확대하면 두번째 버튼이 오른쪽에, 축소하면 아래로 내려간다. - Border : setLayout클래스에서 기본적으로 사용되는 레이아웃, 동서남북 방향으로 배치한다. 파라미터로 int, int가 오면 여백을 의미한다.
4. 인스턴스화 분리 - 선언 : A a = null; - 생성 : a = new A;

### 인스턴스화 위치

* class 밑에 멤버변수로 선언 하면 모두가 가져다 쓸 수 있다.
* 메서드나 생성자 안에서 선언하면 해당 영역 안에서만 사용이 가능하다.
* 공통으로 사용해야하는 인스턴스 변수는 멤버변수로 선언하고, 사용되는 위치 전에 생성해주어야 NullException오류가 나지 않는다.
* 참고 : InstanceTest.java

### 상속

* parent 클래스의 것들을 child클래스가 사용할 수 있다. = child클래스가 영역이 더 넓다.
* 참고 : Father, Son.java

### 추상클래스

* 클래스에 extends를 붙여 쓸 수 있다.
* 상속의 개념을 갖는다. 기존 클래스가 child, 불러온 추상클래스가 parent가 된다.
* child의 범위가 더 크므로 parent 주소번지 = new child\(\);가 성립한다.
* 결합도가 높고, 자유롭지 못하다. 
* 참고 : Duck.java - MallardDuck, WoodDuck, RubberDuck클래스 들이 Duck추상클래스를 parent로 상속한다.

### 인터페이스

* 클래스에 implements를 붙여 쓸 수 있다.
* 인터페이스는 자기안에 메서드를 선언만 할 수 있다.
* 그래서 구현체 클래스가 반드시 필요하다.
* implements하면 override가 자동으로 따라온다.
* 한 클래스 안에서 구현도 하고 사용도 해도되지만 구현체 클래스를 따로 만들어서 빼서 쓰는게 더 편리하다.
* 결합도가 낮다, 비교적 자유롭다.
* 참고 : FlyBehavior.java - FlyWithWings, FlyNoWay 클래스들이 FlyBehavior의 구현체 클래스들이다.

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

* 26번은 상속관계시에만 성립한다.
* 출력결과 : Son ---&gt; methodA 호출성공

### 클래스 안의 클래스

* 클래스 안에서 클래스 선언이 가능하다.
* 안에 있는 클래스를 내부 클래스 라고 한다.
* 반드시 외부 클래스를 인스턴스화 해야만 접근 가능하다.
* 내부 클래스는 외부 클래스의 멤버변수들을 사용할 수 있다.

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

* Inner클래스의 go메서드를 호출하는 방법
* 24, 5-8번 : 생성자이용
* 24-26 : 인스턴스화이용

후기 : 인스턴스화 생성 위치에 따라 null오류가 날수 있으니 주의해야한다. 생성자와 추상클래스, 인터페이스를 더 활용해보도록 하자!

