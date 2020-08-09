---
description: 2020.08.03 - 4일차
---

# 4 Days - 메소드호출, static, 형전환

### 3일차 복습

1. 클래스 구성요소 두가지
   * 변수 : 명사형
   * 메소드 : 동사형
2. 변수선언
   * 타입 이름;
3. 변수이름을 직관적으로 써야하는 이유
   * 부르기 쉽기때문
4. 클래스 란 
   * 객체 : 추상적 \(자동차\)
   * 클래스 : 직관적 \(Sonata\)
   * 추상적인 객체를 직관적, 구체적으로 바꿔주는 것.
5. 변수와 메소드를 구분할 수 있다.

   * 변수 : 이름
   * 메소드 : 이름\( \){ } : 구현, 선언

                          이름\( \){ }; : 호출, 이용, 재사용

6. 파라미터
   * 메소드이름\(파라미터\){ }
   * 두 개 이상 올 수 있다. 모든 파라미터가 맞아야한다.\(and\)
   * 로그인 할때 ID and PW
   * ID와 PW는 변수
   * 파라미터에는 사용자가 입력하는 값이 들어간다. 두 개 이상일 수 있다.

### 학습목표

* 메소드의 파라미터에 선언되는 변수가 무엇인지 설명할 수 있다.
* 메인메소드에 클래스 안의 메소드를 호출 할 수 있다.

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261 : Oracle.com
* 사용Tool : Eclipse : Eclipse.org

### 변수와 인스턴스화 이용하기

```java
package book.ch3;

public class SungJuk {
	/*********************************************
	 * 홍길동 학생의 국어, 수학, 영어 점수를 파라미터로 받아옵니다.
	 * hap메소드를 설계할때, 리턴타입을 int로 한 이유는 avg메소드에서
	 * 재사용하기 위함이다.
	 * @param jumsu1 = 75
	 * @param jumsu2 = 80
	 * @param jumsu3 = 90
	 * @return int
	 *********************************************/
	int hap(int jumsu1, int jumsu2, int jumsu3) {
		int sum;
		sum = jumsu1 + jumsu2 + jumsu3;
		return sum;
	}
	double avg(double result) {
		double answer;
		answer = result/3;				
		return answer;		
	}

	public static void main(String[] args) {
		SungJuk sj = new SungJuk();
		double result = sj.hap(75, 80, 90);
		
		System.out.println(sj.avg(result));			
		
	}

}

```

* 사용 변수: hap, sum, avg, answer, result
* 13번의 hap메소드는 리턴타입은 int이고, 세 파라미터의 합인 sum을 변수를 갖고있다.
* 18번의 avg메소드의 리턴타입은 double이고,  result변수의 /3을 한 answer변수를 갖고있다.
* int hap메소드의 파라미터와 변수들은 모두 int타입, double avg메소드의 파라미터와 변수들은 모두 double타입이다.
* static main메소드에서 클래스를 인스턴스화하여 hap메소드에 값을 지정하여 이를 avg메소드에 대입하였다.
* 결과출력 : 81.0

### static 클래스에서의 메소드 호출

static variable == class variable == 정적변수  
static 메소드는 JDK에서 제공하는 메소드이고 그 외에는 사용자가 정의하는 메소드이다.  
public : 공유하는,+  
private : 비공유하는,-  
public &gt; protected &gt; friendly &gt; private  
접근제한자 / static / 타입 / 메소드이름 / \(파라미터\) ex\)\(String arg\[배열\]\)

```java
package book.ch2;
//주석은 실행되지 않습니다.
//주석에는 업무에 대한 내용과 담당자 이름, 버전정보 등 회사 고유 업무내용이 포함된다.
//따라서 배포할땐 XXX.class만 배표해야한다.
public class Variable2 {
	//전역변수의 위치. 지금 이 클래에 전역변수는 없다.
	int i;//전변생성, 기본값으로 0을 갖는다.
	/**********************************************
	 * 로그인 버튼을 누르면 이 메소드를 호출합니다.
	 * @param id - 사용자가 입력하는 값을 받는다.
	 * @param pw - 사용자가 입력하는 비번을 받는다.
	 * @return - id와 pw를 비교해서 모두 일치하면(교집합)
	 * 	 * 
	 * 내 안에 있는 메소드 메인메소드에서 호출 하려면
	 * 인스턴스화한 후 인스턴스변수.login("apple","123"); : 메소드호출
	 * 회원가입 - 등록 - 오라클
	 * 로그인 - 조회(찾기)
	 **********************************************/
	String login(String id, String pw) {
		return "홍길동님 환영합니다.";
	}
	void methodA(int i){//i는 지변. methodA안에서만 접근 가능하다.
		System.out.println(i);//i는 지역변수
		//내 안에 있는 메소드는 인스턴스화 없이 호출 할 수 있다.
		login("haha","123");//메소드 호출
	}
	void methodB() {
		System.out.println(i);
	}
	
	public static void main(String[] args) {
		//Variable1 v2 = new Variable1();//Variable1에는 login메소드가 없다.
		Variable2 v2 = new Variable2();
		v2.login("haha","123");//메인메소드에서는 인스턴스화 필수
		v2.methodA(5);
		v2.methodB();
		System.out.println("전역변수 i : "+v2.i);

	}

}
```

* class밑의 int i 는 인스턴스변수로, 기본값 0을 갖는다.
* methodA의 파라미터 i는 지역변수이다.
* methodA는 메인 메소드가 아니기때문에 login메소드를 바로 호출 할 수 있다.
* main 메소드는 다른 메소드를 호출하기 위해 인스턴스화가 필요하다.
* 메인메소드에서 인스턴스변수 i를 호출하는 법 1. 27-29번, 36번 처럼 인스턴스변수i를 갖는 메소드를 만든다. 2. 37번 처럼 인스턴스화를 이용하여 바로 호출할 수 있다.
* 35번 에서 methodA는 파라미터로 int i를 갖기때문에 정수 값을 넣어주어야 실행된다.
* 출력결과 5 0 전역변수 i : 0

```java
package book.ch2;
/*
 * 하나의 클래스 안에 여러개의 메소드를 선언할 수 있다.
 * 내안의 메소드는 인스턴스화 없이도 호출할 수 있다.
 * 단 내안에 있는 메소드일지라도 static영역에서 호출할땐 반드시 인스턴스화 해야한다.
 */
class B1{
	void go() {//메소드1
		//home();//B1클래스에서는 B클래스가 소유한 home메소드를 불러올 수 없다.
		//해결 : 소유하고 있는 클래스의 이름으로 인스턴스화 하면 된다.
		B b = new B();
		b.home();
		System.out.println("go호출 성공");
		
	}
	
}
public class B {
	void home() {//메소드2
		System.out.println("home호출 성공");
	}
	void come( ) {//메소드3
		
	}

	public static void main(String[] args) {
		B b = new B();
		B1 b1 = new B1();
		b1.go();
		b.home();
		b.come();

	}

}

```

* 한 클래스 안에는 여러 메소드를 선언할 수 있다.
* 9번은 다른 클래스의 메소드는 인스턴스화 없이는 불러 올 수 없기때문에 실행불가능하다.
* static main 메소드에서는 다른 메소드를 사용하기 위해서는 인스턴스화가 필요하다,
* 출력결과 home 호출 성공 go호출 성공 home호출 성공

### 형전환, 타입 변환

```java
package book.ch2;

public class B {	

	public static void main(String[] args) {
				
		int val = Integer.parseInt("10");
		double dval = Double.parseDouble("3.14");
		String sval = "10";
		int val2 = Integer.parseInt(sval);
		
		System.out.println(val);//문자를 숫자로 출력
		System.out.println(dval);//문자를 숫자로 출력
		System.out.println(sval);//문자출력
		System.out.println(val2);//문자를 숫자로 출력

	}

}

```

* 타입변환 string타입의 문자를 int타입의 정수로 변환시켜주는 등..타입을 변환시켜준다. 자바에서 데이터 손실이 일어나는 변환은 실행되지 않는다.\(큰 자료형에서 작은 자료형으로\)
* 예약어 : parse타입
* 7번에서 int타입 val 변수는 문자인 "10"을 int타입으로 변환시켜 정수 10을 값으로 갖는다.
* 8번에서 double타입 dval변수는 문자인 "3.14"를 double타입으로 변환시켜 실수를 값으로 갖는다.
* 9번에서 String타입 sval변수는 문자 "10"을 값으로 갖는다.
* 10번에서 int타입 val2변수는 위의 sval변수의 값을 int타입으로 변환시켜 정수의 값을 갖는다.
* 결과출력 10 : 숫자 3.14 : 숫자 10 : 문자 10 : 숫자

### JOption.showInputDialog 예약어와 타입변환

```java
package book.ch2;

import javax.swing.JOptionPane;

//non-static변수나 메소드는  static영역에서 사용할 수 없다. <인스턴스화필요
public class Addition {

	public static void main(String[] args) {
		//사용자에 의한 첫번째 문자열 입력
		String firstNumber = "5";
		firstNumber = JOptionPane.showInputDialog("첫번째 숫자 입력하세요.");
		//사용자에의한 두번째 문자열 입력
		String secondNumber="10";
		int number1;//첫번째 숫자 추가
		int number2;//두번째 숫자 추가
		int sum;//number1과 number2 더하기
		//사용자 스트링으로부터 첫번째 숫자 읽기
		//?=Integer.parseInt(firstNumber);
		//string이라 문자가 들어는 가지만 숫가로 변환되지는 않는다=실행에러
		number1=Integer.parseInt(firstNumber);
		number2=Integer.parseInt(secondNumber);
		sum = number1 + number2;
		System.out.println(firstNumber);
		System.out.println(firstNumber+5);
		System.out.println(sum);
		//결과를 나타내기
		JOptionPane.showMessageDialog(null, "The sum is"+sum,"처리 결과", JOptionPane.INFORMATION_MESSAGE);

	}

}

```

* return type 정하기 : 응답 값
* parameter type 정하기 : 사용자가 입력하는 요청 값
* 10-11번에서 String타입 firstNumber변수는 인풋창에 입력되는 값을 문자 형태로 갖는다.
* 13번에서 Stirng타입 secondNumber변수는 문자 "10"으로 초기화한다.
* 20-21번에서 위의 두 변수의 String타입을 int타입으로 변환한다.
* 결과출력 \(firstNumber을 5로 입력했을때\) 5 10 15

### 예시를 만들어보자

#### 입력값을 세번 곱하여 그 값을 창에 띄워주는 코드

```java
package book.ch2;

import javax.swing.JOptionPane;

//non-static변수나 메소드는  static영역에서 사용할 수 없다. <인스턴스화필요
public class Addition2 {

	public static void main(String[] args) {
		
		String firstNumber = JOptionPane.showInputDialog("계산할 숫자 입력하세요.");
		
		int number1=Integer.parseInt(firstNumber);
		int tot = number1*number1*number1;
		
		System.out.println(tot);
		JOptionPane.showMessageDialog(null, "The sum is"+tot,"처리 결과", JOptionPane.INFORMATION_MESSAGE);

	}

}

```

#### 입력값 3개를 곱하여 그 값을 창에 띄워주는 코

```java
package book.ch2;

import javax.swing.JOptionPane;

//non-static변수나 메소드는 static영역에서 사용할 수 있다.

public class Addition3 {

	public static void main(String[] args) {
		int x, y, z, result;
		String xVal, yVal, zVal;
		xVal = JOptionPane.showInputDialog("첫번째 숫자 입력해주세요.");
		yVal = JOptionPane.showInputDialog("두번째 숫자 입력해주세요.");
		zVal = JOptionPane.showInputDialog("세번째 숫자 입력해주세요.");
		
		x = Integer.parseInt(xVal);
		y = Integer.parseInt(yVal);
		z = Integer.parseInt(zVal);
		
		result = x*y*z;
		
		System.out.println(result);
		//결과를 나타내기
		JOptionPane.showMessageDialog(null, "The result is"+result, "처리결과", JOptionPane.INFORMATION_MESSAGE);
		//자바 가상 머신과의 연결고리는 끊어버림
		System.exit(0);

		
	}

}

```

system.exit\(0\);은 자바 가상머신과의 연결고리를 끊는\(==0\)역할을 한다

#### 입력된 두 숫자를 더하고 그 값을 출력하는 코드

```java
package book.ch2;

import javax.swing.JOptionPane;

public class C {

	public static void main(String[] args) {
		String s = JOptionPane.showInputDialog("첫번째 숫자를 입력하고 엔터치기");
		System.out.println(s);
		String s2 = JOptionPane.showInputDialog("두번째 숫자를 입력하고 엔터치기");
		System.out.println(s2);
		
		int num1 = Integer.parseInt(s);
		int num2 = Integer.parseInt(s2);
		
		int sum = num1 + num2;
		
		System.out.println(sum);
		System.out.println(s+s2);				

	}

}
```

###  예제 문제

달의 중력은 지구 중력의 17%에 불과합니다. 지구에서 몸무게가 100kg인 사람은 달에 가면 17kg밖에 안됩니다. 몸무게 N은 실수이고 키보드로부터 입력받습니다.

```java
package book.ch2;

import javax.swing.JOptionPane;

public class D {

	public static void main(String[] args) {
		String s = JOptionPane.showInputDialog("지구에서의 몸무게를 입력하시오");
		System.out.println("지구에서의 몸무게 : "+s+"kg");
		
		double num1 = Double.parseDouble(s);
		
		double moon = num1*0.17;
		
		System.out.println("달에서의 몸무게 : "+moon+"kg");				


	}

}

```

### 강제 형전환

```java
package book.ch2;
/*
 * 대입연산자 오른쪽에 있는 값을 왼쪽에 대입한다.
 * 주의사항
 * 작은 값을 더 큰 타입에 대입하는 건 합법.
 * 그러나 큰 타입을 작은 타입의 변수에 대입하는 건 불법이다.
 */

public class E {

	public static void main(String[] args) {
		int i = 1;
		double d = i;
		//i = d;//double의 범위가  int보다 넓으므로 성립되지않는다.
		//강제 형전환, 왼쪽에 대입하므로 왼쪽 타입으로 강제형전환 하도록 한다.
		i = (int)d;
		d = i;
		float f = 1.5f;
		i = (int)f;
		float f1 = (float)d;
		
		System.out.println(i);//1
		System.out.println(f1);//1.0

	}

}

```

* a항 = b항 : 대입연산자의 오른쪽에 있는 값을 왼쪽에 대입한다.
* 14번처럼 큰 타입의 변수를 작은 타입의 변수에 대입하는건 오류가 날 수 있다.
* 강제 형전환 : 변수 = \(전환타입\)변수이름;
* i는 int타입으로 정수 1값이고, d는 double로 실수 값을 갖기 때문에 12, 13번이 성립한다.
* 16번에서 i는 int로 변환되어 소수점이 없는 1을 값으로 갖는다.
* 17번에서 d는 정수 1을 값으로 갖는다.
* 19번에서 i는 int로 변환되어 소수점이 없는 1을 값으로 갖는다.
* 20번에서 f1은 float으로 변환 d의 1값을 갖는다.
* 결과출력 1 1.0 \(float는 실수타입으로 소수점을 갖는다.\)
* 참고 : [https://blog.naver.com/pdh6941/221771802609](https://blog.naver.com/pdh6941/221771802609)

### 다음시간 맛보기

```java
package book.ch2;

public class E2 {

	public static void main(String[] args) {
		//배열이름의 주소번지와 첫번째 방의 주소번지가 같은 값이므로
		//같은 타입의 값을 여러개 담을 수 있는 변수  args는 0번부터 사용하게되었다.
		System.out.println(args[0]);
		System.out.println(args[1]);
		System.out.println(args[2]);
		int j = 0;
		System.out.println(args[j++]);//0
		System.out.println(args[j++]);//1
		System.out.println(args[j++]+10);//2

	}

}

```

* arg는 0부터 시작한다.
* Run as - Run Copiguration - 메인클래스 선택 - Arguments

후기 : 4일차에는 선생님이 주신 예제문제도 잘 풀었다. 앞으로도 주신 코드를 바탕으로 많은 시도를 하면서 공부를 해야겠다는 생각이 든다.

