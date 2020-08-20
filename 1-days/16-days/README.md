---
description: 2020.08.20 - 16일차
---

# 16 Days - privat, static, API활용

기 : private변수, public메서드, 재사용성\(함수의장점\), 지변의 유지  
FunctionExam1.java : 메서드 오버로딩, 생성자, 전변,

### **복습**

* 지역변수\(Automatic\) - stack에 저장된다. - FILO성격을 갖는다.\(first in last out\) - 지변이 사라지는때 : 함수가 호출되어 실행문이 끝났을때. 메서드 밖에서 유지되지 않는다. - 인스턴스화를 통해 지역변수를 사용할 수 있다. - 인스턴스화를 메서드 안에서 하면 되부 메서드에서는 사용할 수 없다.

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261 : Oracle.com
* 사용Tool : Eclipse : Eclipse.org

### Private 멤버변수와 public 메서드

#### Public메서드로 풀어줄거면 왜 private로 선언을 하는가?

```java
private NichName;

void m() {
 MemberVO mVO. = new MemberVO();
 mVO.setNickName("apple");
 String NickName = mVO.getNickName( ); } 
```

* 인스턴스 변수 mVO는 메서드를 사용하는 사람에 따라 다른 값을 담아야 한다. = 종료되어야한다. = 지역변수로 선언해야한다.
* mVO는 m메서드 밖에서는 유지되지않는다. 메서드가 끝나면 사라진다.  - 사용하고 나면 사라지므로 안전하다.
* NickName은 동시접속자들을 고려하여 멤버변수로 선언해야한다.
* NickName은 private변수이지만 set, get메서드는 public이다.
* 5번 : mVO는 set의 파라미터를 통해 "apple"을 담는다.
* A : 안전성을 고려하여 private선언해야한다. 찾는 "사람"이라는 양식이 변하면 안되므로

### 함수\(메서드\)의 생성과 호출

* level 1 : void 메서드이름\(\) {}
* level 2 : int 메서드이름\(\)return int;{}
* level 3 : 클래스 메서드이름\(\)return;{}
* 접근제한자 - private 함수 : 해당 클래스에서만 사용될 수 있다. - public 함수 :  다른 클래스에서 사용할 수 있다.
* 참고 : FunctionExam1.java

### 멤버변수와 지역변수 구분

* static으로 선언된 멤버변수 : 다른 클래스, 함수에서 사용할 수 있다.
* 클래스 밑에서 멤버변수로 초기화된 변수 : 다른 클래스, 함수에서 사용할 수 있다.
* 참고 : LocalVariable.java

### Static

* **싱글톤** : 단 하나만 생성되는 객체. 하나의 객체를 여러 함수나 클래스가 나눠 쓴다 - 호출이 반복되어도 실제로 생성되는 객체는 최초 생성된 객체를 반환하는 것이다. - db를 연동하는 클래스에서 주로 사용되는데 이는 메모리를 절약하기 위함이다. - static을 사용한다.
* **static** - static이 붙은 변수나 함수\(메서드\)를 싱글톤이라고 한다. 고정된 단 하나의 원본개념.
* non-static영역에서는 static타입 변수를 사용할 수 있다.
* static 영역에서는 non-static 영역은 사용할 수 없다.  - 인스턴스화를 통해야한다.
* 참고 : LocalVariable.java

### LocalVariable.java

```java
package book.ch6;
/*
 * methodA에 선언된 지변을 외부 다른 메서드에서 유지 또는 사용할 수 있나요?
 * 1)static으로 선언된 변수를 사용하는것.(전역변수이용)
 * 2)전변에서 초기화하기. this.i = i;(전역변수이용)
 * 사용불가합니다. static이나 전역변수만 가능해요
 */
public class LocalVariable {
	int i = 1;
	//static int i = 2;//i가중복선언 되어있기때문에 이름을 다르게 해야한다.
	static int j = 2;//=싱글톤이다.
	
	void methodA() {
		int i;
		i=10;
		System.out.println(i);//10
		//static타입의 변수를 non-static영역에서 사용 가능
		System.out.println(j);//2
		
		this.i = i;//지역변수 i=10을 전역변수에 대입한다.
	}	
	//static영역에서 non-static영역은 사용불가, 인스턴스화하면 사용가능
	public static void main(String[] args) {
		//메인메서드에서 지역변수 i는 접근이 불가하다.
		LocalVariable lv = new LocalVariable();
		System.out.println(lv.i);//인스턴스변수로 접근가능한 변수는 전역변수 뿐이다. = 1출력
		System.out.println(j);//2출력
		j = 50;
		lv.methodA();//j=2->50
		System.out.println(lv.i);//위 methodA에서 전역변수가 10으로 변했으므로 10출력
	}
}
```

### Company.java-private static

private : 해당 클래스에서만, static : 싱글톤, 단 하나의 원본

```java
package book.ch6;

public class Company {
	
	private static Company instance = null;//new Company();
	
	private Company() {
		System.out.println("Company 디폴트 생성자 호출성공");
	}
	
	public static Company getInstance() {
		if(instance == null) {//인스턴스선언만 되어잇니?
			instance = new Company();//그럼 생성
		}			
		return instance;
	}
}
//private는 데이터를 get으로 걸러 받을때, 조건을 걸어야할때 사용
```

```java
package book.ch6;

public class CompanyTest {

	public static void main(String[] args) {
		//Company cp = new Company(); //생성자가 private이므로 호출불가
		Company cp1 = Company.getInstance();//static이므로 : 타입.메서드();
		Company cp2 = Company.getInstance();//7번에서 인스턴스가 생성되었으므로 null이 아니다. 생성자를 호출하지 않는다.
		System.out.println(cp1 == cp2);//싱글톤이므로 주소가 두개지만 하나를 바라보고있다. true이다.
	}
}
```

