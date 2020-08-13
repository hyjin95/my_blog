---
description: 2020.08.11 - 10일차
---

# 10 Days - 객체 배열, 메서드중복정의, 생성자

### 복습

* **패키지와 import -** 패키지는 폴더의 개념으로 클래스 파일 묶음이다. - 기본적으로 java.lang이 있다. - java.lang 패키지는 toString 클래스를 갖고있다. - 사용자가 정의한 클래스 이더라도 패키지가 다르면 반드시 import해주어야 한다. -class 이름이 서로 같은 class이더라도 각각 해당하는 패키지가 다르다면 서로 다른  class이다.
* **메서드 -** 함수의 종류로, 동사의 개념으로 기능을 수행하는 함수이다. - 사용자의 입력 : 로그인, 주문 등.. == 요청의 개념 \_시스템의 반환 : == 응답의 개념 - 요청 -&gt; 응답 -&gt; 요청 -&gt; 응답 -&gt; ...  == 반복문
* **return 예약어, 반환형 -** int나 double, boolean등 과 같이 return값이 필요한 타입을 반환형이라고 한다. - return은 클래스 안에서 두가지 쓰임새로 사용될 수 있다. - if문의 break로서의 return : 메서드를 빠져나가는 흐름제어의 역할 - 메서드에서의 return : 응답의 의미로, 메서드를 재사용할수 있게 해준다. - 타입 void는 return값이 필요없다.
* **main함수** - ****==main Thread  - thread : 첫번째가장 먼저 실행되는, 프로그램을 실행하는 함수이다. - 동시접속 기능이 필요하다면 필수 함수이다. - 한정된 자원을 같이 사용해야 할때. - ex\) 네비게이션, 게임, ...

### 낙타 표기법

* 자바에서 이름을 지을때 필요한 규약이다.
* 클래스 이름은 대문자로 시작한다.
* 패키지, 변수, 메서드 이름은 소문자로 시작하되 중간에 새 단어가 들어가면 대문자를 사용해 가독성을 높인다.

### 객체\(VO\) 배열

1. DeptVO dVO = new DeptVO\( \);        : Dept=부서, DeptVO=클래 - deptno : int        : 변수1, 부서번호 : 10 - dname : string  : 변수2, 부서이름 : Accounting - loc        : string  : 변수3, 부서위치 : NEW YORK
2. dVO = new DeptVO\( \); - deptno : 20 - dname : Rsearch - loc : DALLARS

* VO == Value Object
* 변수는 1타입, 1값만 가질 수 있다. = 중복정의 될 수 없다.
* 1에서 값을 가진 3개의 변수는 2에서 새로 정의된다. 원래 값이 날아가버리기 때문에 이를 안전하게 담아두는 것이 배열이다.
* 객체배열 = 객체를 담은 배열 : 클래스, 메서드 등..
* 배열이 두 칸 있으면 첫번째 칸은 1번 dVO 객체를, 두번째 칸은 2번 dVO객체를 담는다.
* java에서의 class는 oracle에서 Table이다.
* 학생이 버스를 탄다. - 학생 : 객체 &lt;---&gt; 버스 : 객체 - 학생이라는 class에서 버스라는 객체를 인스턴스화 하여 가져온다. - 학생이 내리면 버스 객체는 종료된다. - 버스는 정해진 노선이 있는 메서드 개념이다. - 학생은 한명이 아니므로 객체 배열을 써야한다. 
* DeptVO\[ \] dVOS = new DeptVO\[4\]; - DeptVO타입의 dVOS이름을 갖는 객배열은 방이 4칸이다. - 각 방에는 DeptVO타입인 dVO에 해당하는 값들이 들어있다. - dVOS\[0\] =dVO, dVOS\[1\] =dVO, dVOS\[2\] =dVO, dVOS\[3\] =dVO - 다 주소가 다른 dVO이기때문에 담고있는 값이 다르다. - a는 a의 행\(a의 이름, a의 학번, a의 주소\) - b는 b의 행\(b의 일름, b의 학번, b의 주소\)를 담는다는 뜻이다. 

### 이중배열

* String data\[ \] \[ \] = new String\[2\] \[3\];

| 0,0 | 0,1 | 0,2 |
| :---: | :---: | :---: |
| 1,0 | 1,1 | 1,2 |

* 2행, 3열의 배열 생
* 변수의 갯수 : 2x3 = 6
* 위의 table과 같이 이중배열인 경우 좌표값으로 두개의 값이 필요하다.
* 일반 배열은 for문으로 끝난다. 방안의 값이 하나이므
* 이중 배열은 for\(for\(\)\)문을 사용한다.  - 방안의 값이 두개이므로  for\(\){첫번째 값 for\(\){두번째 값}} 으로 구한다.

### 메서드 중복정의

* 변수와는 다르게 메소드는 중복 정의 될 수 있다.
* **overloading** - 같은 이름의 메소드를 n개 만들때,  - 반드시 파라미터의 갯수나 타입이 달라야한다. = \|\| - 리턴 타입의 유무, 예외처리의 유무, 접근제한자의 유무, 범위가 다른 것은 영향이 없다.
* **overriding** - 같은 이름의 메소드를 n개 만들때, - 반드시 상속관계에 있을 때에만 허용된다. - 부모 클래스의 원형을 건드리면 안된다. = 파라미터, 리턴의 타입을 수정할 수 없다. - 기능에 대한 구현 부분만 수정하여 사용한다.
* 공통점 - 메소드들이 같은 이름일때만 허용된다.

### 생성자

* 모든 클래스들은 자기안에 클래스이름과 같은 이름의 생성자를 갖는다.
* 이 생성자는 초기화를 대신 해주는 역할로,  표시 생략이 가능하다.
* 생성자 또한 메서드로서 overloading, overriding이 가능하다.

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
	//파라미터는 사용자에게 입력받는 값으로 입력된 정보를 활용하여 응답할수있다.
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

### 디폴트 생성자 이해하기

```java
package book.ch5;

import javax.swing.JButton;
import javax.swing.JFrame;
//오늘부터는 되도록 메인 메소드에 코딩하지 않는다.
//한번에 어렵다면 조금씩 부피를 줄여보자.
//생성자는 의존관계를 실제로 표할 수 있다. - 클래스 쪼개기 연습시 꼭 활용해 볼 것.
//static은 사용하지 말것
//초보자가 static에 for,if에 break;, while문을 증감연산자 없이 사용해서 다운되는 경우가 많다.
//반드시 무한루프 방지 코드를 작성하자!
public class DeptManager extends JFrame {//extends : 부모
	int deptno;
	
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

* 27번은 22번 생성자 메소드를 overloading한 메소드이다.
* 55번은 같은 파라미터를 갖는 27번 생성자 메소드를 값에 의한 호출하는 구문이다.
* 27번의 deptnos는 10이다. 28번의 this.deptnos는 전역변수를 불러온 것으로 10이 대입된다.
* 29번에서 this.deptnos는 30으로 초기화 된다. 여전히 deptnos는 10이다.
* 57번에서 새로 인스턴스화를 하여 22번 생성자가 호출된다.
* 58번에서 dm이라는 주소를 가진 methodA를 호출하는데 이때 deptnos는 전역변수값을 갖는다.
* 59번에서 dm이라는 주소를 가진 deptno을 출력한다.
* 결과출력 디폴트 생성자 30 10 ---------------------------- 디폴트생성자 ---------------------------- methodA에서 0 ---------------------------- 0

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

* java.lang이라는 패키지는 toString 메소드를 기본으로 갖고 있는다.
* 6-7번이 없으면 12번에는 주소 가 출력되지만, 6-8번에서 생략가능 했던 값이 없는 toString에 반환 값을 주었기 때문에 "그녀의 자동차"가 출력된다.
* 12번 13번은 같은 의미로, toString은 생략해도 호출된다.

### 객체 배열 사용하기

```java
package book.ch5;

public class DeptVOSimulation {

	public static void main(String[] args) {
		//부서번호 10, 부서이름 accounting, 부서위치 newyork
		DeptVO dVO = new DeptVO();//한번에 하나의 로우를 담는다.
		DeptVO[] dVOS = new DeptVO[3];
		
		dVO.setDeptno(10);
		dVO.setDname("Accounting");
		dVO.setLoc("NEW YORK");
		System.out.println(dVO.getDeptno());
		System.out.println(dVO.getDname());
		System.out.println(dVO.getLoc());
		
		dVOS[0] =dVO;//풍선을 넣어둠
		
		dVO = new DeptVO();//변수이름은 같아도 주소는 다르다. 복사, 이사 : 한번에 하나의 로우를 담는다.
		dVO.setDeptno(20);
		dVO.setDname("RESEARCH");		
		dVO.setLoc("DALLAS");
		System.out.println("===============");
		System.out.println(dVO.getDeptno());
		System.out.println(dVO.getDname());
		System.out.println(dVO.getLoc());
		
		dVOS[1] =dVO;//20이 담긴다

		dVO = new DeptVO();//주소 복사, 이사 : 한번에 하나의 로우를 담는다.
		dVO.setDeptno(30);
		dVO.setDname("SALES");		
		dVO.setLoc("CHICAGO");
		System.out.println("===============");
		System.out.println(dVO.getDeptno());
		System.out.println(dVO.getDname());
		System.out.println(dVO.getLoc());
		System.out.println("===============");

		dVOS[2] =dVO;//30이 담긴다.
		//length가 있으면 원소의 갯수를 적어준다. = 세방에 원소가 3개, 3
		for(int i=0;i<dVOS.length;i++) {
			DeptVO rVO = dVOS[i];
			System.out.println(rVO.getDeptno()+", "+rVO.getDname()+", "+rVO.getLoc());
		}
	}//위의 부서번호,이름, 위치는 고정 값 
	//값이 수정되었을때 죽은 값이 나오는 오류가 날 수 있어서 이런방법을 선호하지 않는다.
	//부서가 여러 개인 경우에 이런방법은 쓸 수 없다.

}//Eclipse에서 debug모드로 변화를 확인할 수 있다.

```

* 8번에서 DeptVO\[ \]는 dVOS주소를 갖는 방 3개짜리 배열이다.
* 첫번째 방인 DeptVO\[0\]은 10-12번의 dVO 값을 담다.
* 두번째 방인 DeptVO\[1\]은 새로 정의된 20-22번의 dVO 값을 담는다.
* 세번째 방인 DeptVO\[2\]는 새로 정의된 31-33번의 dVO 값을 담는다.
* 42번 for문은 방의 갯수만큼 반복한다.
* 첫번째 출력은 DeptVO\[0\]안의 값들을, 두번째 반복출력은 DeptVO\[1\]안의 값들을, 세번째 반복출력은 DeptVO\[2\]안의 값들에서 출력된다.
* Deptno, Dname, Loc는 이미 고정된 값들로, 이 값을 수정하면 죽은 값이 나오는 오류가 발생하기도 하고, 부서가 많아질수록 이런 방법은 비효율적이다.

### 객체 배열과 for 문의 사용

```java
package book.ch5;

import java.awt.GridLayout;

import javax.swing.JButton;
import javax.swing.JFrame;

public class ObjectArray extends JFrame{
	JButton jbtns[] = new JButton[10];//JButton은 string 타입이다. >19번
	int     nums[]   = {0,1,2,3,4,5,6,7,8,9};
	
	public ObjectArray() {
		initDisplay();
	}
	
	public void initDisplay() {
		this.setLayout(new GridLayout(1,10));
		
		for(int i=0;i<jbtns.length;i++) {
			jbtns[i]= new JButton(nums[i]+"");//버튼만들기 반복 / =객체배열 / 괄호안에 String 타입이 와야하기때문에 ""를 더한다.
			this.add(jbtns[i]);//담는 부분도 반복
		}
		this.setSize(600,200);
		this.setVisible(true);
	}

	public static void main(String[] args) {
		new ObjectArray();
	}
}

```

* 9번에서 jbtns이름의 버튼은 10개의 방을 갖는 배열로 정의된다.
* 19번-21번 처럼 버튼만들기와 버튼이 담는 값을 반복해서 정의할 수 있다.
* 각 버튼을 따로 지정하는 것보다 편리하다.

### java와 Oracle 연결

1. 연결통로 만들기 - ==드라이버 로 - oracle에 연결하려면 orcleDriver.class가 있는지 확인한다.
2. 메모리에 oracleDriver.class올리기 - 올리기 위해서 ID, PW, IP가 필요하다 - java 에서 Connection클래스를 이용한다. - java database connection == jdbc

* Oracle 일꾼 : 옵티마이저
* java     일꾼 : JVM
* Oracle : Curser - SELECT문에서 커서가 있는 자리에 값이 있니? -있으면 true, 없으면 false를 반환하는 논리형 메서드이다. false는 break의 개념으로 빠져나온다.
* Java : Resultset - java에서 oracle의 SELECT에 대응하는 인터페이스이다. - SELECT문의 Curser를 조작해서 다음으로 이동해서 는 조작을 하는 인터페이스이다. - ==rs.next\(\); : boolean타입 : 다음으로 가는 조작
* java에서 DB서버와 연결되는 것에 관한 import는 모두 java.sql패키지를 사용해야한다.

후기 : 힘들어도 매일 그날그날 정리하도록 노력하고있다. 미루지 말고 끝까지 가자!

