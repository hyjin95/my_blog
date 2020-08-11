---
description: 2020.08.11 - 10일차
---

# 10 Days - 객체, 메소드중복정의

### 메소드 중복정의 이해하기

```java
package book.ch5;
/*
 * 메소드 overloading
 * 자바에서는 같은 이름의 메소드를 n개 정의할 수 있다.
 * 단, 반드시 파라미터의 갯수나 타입이 달라야 한다. ||합집합
 * 리턴 타입이 있고 없고는 영향이 없다.
 * 예외를 던지고 던지지 않고는 영향이 없다.
 * 접근 제한자가 있고 없고 나 범위가 다른 것도 영향이 없다. 
 * 
 * 메소드 overriding
 * 전제 조건으로 반드시 상속 관계에 있을 때에만 가능하다.
 * 부모 클래스의 원형을 절대로 건드리면 안된다.
 * 파라미터나 리턴 타입을 수정하지 않는다.
 * 기능에 대한 구현만 수정하여 사용하는 것이다.
 * 
 * 공통사항
 * 둘다 반드시 메소드가 같은 이름일때만 고민한다.
 * 메소드 이름이 다른 것들은 고려대상이 아니다.
 */

public class Student {
	void go() {
		System.out.println("가주세요.");
	}
	//파라미터는 사용자에게 입력받는 값으로 사용자가 입력한 정보를 활용하여 응답해 줄 수 있다.
	//사용자의 정보를 듣게 됨으로써 맞춤형 서비스를 제공할 수 있다.
	void go(String location) {
		System.out.println(location+"으로 가주세요.");
	}
	
	void go(String location, int x) {//파라미터의 개수와 타입이 다르다.
		System.out.println(location+" "+x+"번지로 가주세요.");
	}

	public static void main(String[] args) {
		Student s1 = new Student();
		s1.go();
		s1.go("가산동");
		s1.go("가산동", 54);
		//이러한 규칙이 생성자에게도 동일하게 적용된다.
	}
}

```

### 디폴트 생성자

```java
package book.ch5;

import javax.swing.JButton;
import javax.swing.JFrame;
//오늘부터는 되도록 메인 메소드에 코딩하지 않는다.
//한번에 어렵다면 조금씩 부피를 줄여보자.
//생성자는 의존관계를 실제로 표헌할 수 있다. - 클래스 쪼개기 연습시 꼭 활용해 볼 것.
//static은 사용하지 말것
//초보자가 static에 for,if에 break;, while문을 증감연산자 없이 사용해서 다운되는 경우가 많다.
//반드시 무한루프 방지 코드를 작성하자!
public class DeptManager extends JFrame {//extends : 부모
	int deptno;//10->30
	
	//BorderLayout - 안드로이드수업
	JButton jbtn_center = new JButton();
	JButton jbtn_north  = new JButton("북쪽");
	JButton jbtn_south  = new JButton("남쪽");
	JButton jbtn_east   = new JButton("동쪽");
	JButton jbtn_west   = new JButton("서쪽");
	
	//디폴트 생성자의 역할은 초기화를 대신 해주는 것이다. 생략가능
	public DeptManager() {//디폴트 생성자는 생략할 수 있다.생성자메소드1
		System.out.println("디폴트 생성자");
		System.out.println("--------------");
	}
	
	public DeptManager(int deptno) {//생성자메소드2. overloading. deptno는 지역변수이다. : 자식
		this.deptno = deptno;
	    this.deptno = 30;//10->30 재정의 
		System.out.println("디폴트 생성자 "+this.deptno);
		System.out.println(deptno);
		System.out.println("--------------");
		initDisplay();
	}
	
	public void methodA() {//deptno가 이 메소드에서는 없는 값이다. 쓰려면 전역변수로 선언해야한다.
		System.out.println("methodA 에서 "+deptno);
		System.out.println("--------------");
	}
	
	public void initDisplay() {
		jbtn_center.setText("중앙");
		this.add("Center", jbtn_center);
		this.add("North", jbtn_north);
		this.add("South", jbtn_south);
		this.add("East", jbtn_east);
		this.add("West", jbtn_west);
		this.setSize(400, 300);
		this.setVisible(true);
	}

	public static void main(String[] args) {
		JFrame.setDefaultLookAndFeelDecorated(true);//자바 기본 스킨 없으면 윈도우 스킨
			
		new DeptManager(10); //주소번지가 없다. 생성자는 클래스와 이름이 같기때문에 'DeptManager 주소 =' 이 필요없다.
		
		DeptManager dm = new DeptManager();//new 클래스이름==생성자호출
		dm.methodA();
		System.out.println(dm.deptno);
	}//생성자가 존재하는 이유 = 초기화, 재사용
}
```

### **java.lang.Object 클래스의 기본 메소드 toString**

```java
package book.ch5;

public class Pride extends Object {//모든 클래스의 아버지, toString()라는 메소드를 기본으로 갖고있다.
	int speed = 0;
	
	public String toString() {//클래스에 포함된 기본 메소드
		return "그녀의 자동차";
	}

	public static void main(String[] args) {
		Pride hercar = new Pride();
		System.out.println(hercar);//6-8번이 없으면 주소번지가 찍힌다.
		System.out.println(hercar.toString());//toString 생략가능
	}
}

```

