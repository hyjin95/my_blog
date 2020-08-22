---
description: 2020.08.20 - 16일차
---

# 16 Days - privat, static, API활용

### **복습**

* 지역변수\(Automatic\) - stack에 저장된다. - FILO성격을 갖는다.\(first in last out\) - 지변이 사라지는때 : 함수가 호출되어 실행문이 끝났을때. 메서드 밖에서 유지되지 않는다. - 인스턴스화를 통해 지역변수를 사용할 수 있다. - 인스턴스화를 메서드 안에서 하면 되부 메서드에서는 사용할 수 없다.

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261 : Oracle.com
* 사용Tool : Eclipse : Eclipse.org

## 이론

### Private 멤버변수, public 메서드

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
* A : 안전성을 고려하여 private선언해야한다. 찾는 "사람"이라는 양식이 변하면 안되므로 - = private는 get, set함수와 함께 주로 사용되고, 가져오는 값에 기준을 걸어야 할때에 사용한다.
* 참고 : Company-CompanyTest.java, 15Days - NickNameList - memberVO.java

### 함수\(메서드\)의 생성과 호출

* level 1 : void 메서드이름\(\) {}
* level 2 : int 메서드이름\(\)return int;{}
* level 3 : 클래스 메서드이름\(\)return;{}
* 접근제한자 - private 함수 : 해당 클래스에서만 사용될 수 있다. - public 함수 :  다른 클래스에서 사용할 수 있다.
* 참고 : FunctionExam1.java, Company-CompanyTest.java

### 멤버변수와 지역변수 구분

* static으로 선언된 멤버변수 : 다른 클래스, 함수에서 사용할 수 있다.
* 클래스 밑에서 멤버변수로 초기화된 변수 : 다른 클래스, 함수에서 사용할 수 있다.
* 참고 : LocalVariable.java, Company-CompanyTest.java

### Static

* **싱글톤** : 단 하나만 생성되는 객체. 하나의 객체를 여러 함수나 클래스가 나눠 쓴다 - 호출이 반복되어도 실제로 생성되는 객체는 최초 생성된 객체를 반환하는 것이다. - db를 연동하는 클래스에서 주로 사용되는데 이는 메모리를 절약하기 위함이다. - static을 사용한다.
* **static** - static이 붙은 변수나 함수\(메서드\)를 싱글톤이라고 한다. 고정된 단 하나의 원본개념.
* non-static영역에서는 static타입 변수를 사용할 수 있다.
* static 영역에서는 non-static 영역은 사용할 수 없다.  - 인스턴스화를 통해야한다.
* 참고 : LocalVariable.java, Company-CompanyTest.java

### API활용

* 함수를 사용\(소유\)하려면 인스턴스화를 해야한다. - 생성자의 파라미터의 타입과 갯수를 알아야한다. - API
* 참고 : NickNameList.java+
* Header = 컬럼명 -&gt; String cols\[ \] = {"대화명," "성별"} - 컬럼명을 String타입으로 직접작성해놓았다.
* TableHeader를 사용해야할때, API를 살펴보자.
* Header를 읽어올 수 있는 getTableHeader\( \);함수가 필요하다.
* JTableHeader클래스는 해당 함수를 갖고있지않다. -&gt; JTable함수가 갖고있다.
* getTableHeader함수의 리턴타입은 JTableHeader이다. -&gt; JTable은 양식만 빌려준다.
* JTableHeader의 생성자를 살펴보면 TableColumnModel인데, 이 인터페이스 안에는 String타입의 메서드가 존재하지 않는다. -&gt; 우리가 정해놓은 컬럼명이 String타입이므로 사용할 수 없다.
* JTableHeader jth = new JTableHeader\( \);  X
* **JTableHeader jth = jtb.getTableHeader\(\);  O** - 선언부 : JTable.jtb = new JTable\(\); 인스턴스화 되어있다. - jtb.getTableHeader\(\); = JTable에 있는 getTableHeader\(\)메서드 ;호출



## 코드

### LocalVariable.java

```java
package book.ch6;

public class LocalVariable {
	int i = 1;
	//static int i = 2;//i가중복선언 되어있기때문에 이름을 다르게 해야한다.
	static int j = 2;
	
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
		System.out.println(lv.i);
		System.out.println(j);//2출력
		j = 50;
		lv.methodA();//j=2->50
		System.out.println(lv.i);
	}
}
```

* 6번 : int타입 멤버변수 j는 static이 붙은 싱글톤 변수이다.
* 9번 : int i는 1이다.
* 10번 : i는 10으로 초기화된 지역변수
* 15번 : 멤버변수 i에 지역변수 i의 값10을 대입시킨다.
* 23번 : j는 50으로 초기화된 지역변수
* 24번 : methodA실행, 23번 다음에 실행되어 j는 50을 갖는다.
* **인스턴스변수가 접근할 수 있는 변수는 멤버변수이다.**
* 출력결과 - 21번 : lv.i는 methodA가 실행되기 전의 멤버변수 이므로 1 출력 - 22번 : 2출력 - 25번 : lv.i는 methodA가 실행된 후 멤버변수이므로 10 출력

### Company.java-private static

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
```

* 7번 : private 디폴트 생성자
* 11번 : Company타입인 메서드, 인스턴스변수를 반환한다.

```java
package book.ch6;

public class CompanyTest {

	public static void main(String[] args) {
		//Company cp = new Company(); //생성자 호출불가
		Company cp1 = Company.getInstance();
		Company cp2 = Company.getInstance();
		//7번에서 인스턴스가 생성되었으므로 null이 아니다. 생성자를 호출하지 않는다.
		System.out.println(cp1 == cp2);
		//싱글톤이므로 주소가 두개지만 하나를 바라보고있다. true이다.
	}
}
```

* Company클래스의 디폴트 생성자가 private이므로 다른 클래스에서는 호출이 불가능하다.
* Company의 getInstance메서드가 static메서드이다.  - 타입.메서드이름\( \); 으로 호
* 7번 : 메서드 호출로 인스턴스화가 일어나고 생성자를 호출한다.
* 8번 : 인스턴스화가 이미 일어났으므로 Company의 getInstance메서드가 실행되지 않는다.
* 10번 : 7,8번의 인스턴스변수는 주소가 다르지만 Company의 getInstance메서드가 static이므로 같은 원본을 바라 보고있기때문에 true가 출력된다.
* 출력 결과 : true

후기 : 그날그날 정리할 수 있도록 노력하자 노력하는만큼 실력이 향상될것이다!

