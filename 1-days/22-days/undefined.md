# 문제풀이

### Question 1

```java
package scjp.method;

class A {
	public byte getNumber() {
		return 1;
	}
	
	public short getNumber(int i) {
		return 2;
	}
	
	public double getNumber(double y) {
		return 2;
	}
}

class B extends A {
	/*
	public short getNumber() {//부모메서드의 타입, 파라미터 갯수는 같지만 타입이 다르다. 오버라이딩 오류
		return 2;
	}*/
	
	public short getNumber(int x) {//메서드 오버라이딩 규칙 준수, 타입과 파라미터 갯수가 같다
		return 22;
	}
	
	public short getNumber(int i, boolean x) {//메서드 오버로딩의 규칙을 준수한다. 파라미터 갯수가 다름
		return 3;
	}
}

public class Q1 {

	public static void main(String[] args) {
		A a = new A();
		a.getNumber(3.14);
		a.getNumber(5.0);
		
		a = null;
		a = new B();
		a.getNumber();
		a.getNumber(3.14);
		a.getNumber(7);
		
		//부모 타입으로 선언된 변수는 자식 클래스를 생성했다 해도 부모에 선언된 메서드만 호출 할 수 있다.
		//그렇다면 부모와 자식 모두에 선언된 메서드 중에서 누가 호출될까?
		System.out.println(a.getNumber(7));//자식 클래스의 메서드 호출
		
		B b = new B();
		b.getNumber(4, true);	
	}
}
```

* class B extends A = B is a A = B풍선안에 A풍선
* getNumbr메서드는 B와 A가 상속관계 이므로 오버라이딩 메서드이다.
* A부모의 메서드를 B가 재정의 했다. legal
* 부모의 타입을 바꿔버렸다 - 오류발생

```java
package scjp.method;

import java.util.List;
import java.util.ArrayList;
import java.util.Vector;

import desigin.test.ZipCodeVO;

class QS1 {
	ZipCodeVO zVO = null;
	ZipCodeVO[] zVOS = null;
	
	public void init() {
		List li = new Vector();
		ArrayList al = new ArrayList();
		Vector v = new Vector();
		
		zVO = new ZipCodeVO();
		zVO.setZipcode(20509);
		zVO.setAddress("서울시 금천구 가산동");
		li.add(zVO);
		v.add(zVO);
		
		zVO = new ZipCodeVO();
		zVO.setZipcode(20509);
		zVO.setAddress("서울시 마포구 공덕동");
		li.add(zVO);
		v.add(zVO);
		
		zVOS = new ZipCodeVO[li.size()];
		//li.copyInto(zVOS);//오류
		//al.copyInto(zVOS);//오류
		v.copyInto(zVOS);
	}
}

public class Q1_1 {

	public static void main(String[] args) {
		QS1 qs1 = new QS1();
		qs1.init();
		for(int x=0;x<qs1.zVOS.length;x++) {
			System.out.println("zVOS["+x+"] ---> "+qs1.zVOS[x].getZipcode()+", "+qs1.zVOS[x].getAddress());
		}
	}
}

```

