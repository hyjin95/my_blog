---
description: 2020.18.13 - 12일차
---

# 12 Days - 객체배열과 for문, 생성자의 역할, Git사용

### 복습

* 디폴트 생성자란? - 파라미터가 없는 기본 생성자를 말한다. - 호출방법 : new 클래스이름\(\);
* 클래스의 종류 - 사용자 정의 클래스 : 생성자를 재정의 할 수 있다. \_ 자바 제공 클래스\(API\) : 정해져있어 수정하면 안된다.

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261 : Oracle.com
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5 - Git

### 인스턴스화 와 Call Back메서드

* 내가 하는 인스턴스화
* 남이 하는 인스턴스화\(==객체주입법\) - 자원관리를 외부에서 해준다.
* Call Back 메서드는 Event가 감지돠면 시스템이 호출하는 메서드이다. - 시스템이 호출 == 주입 - ex\) ActionEvent

### API 보는법

* 사용하고자 하는 클래스를 검색하면, 그 클래스의 부모와 사용할수 있는 클래스들을 볼 수있다.
* 하위 클래스는 부모 클래스의 것을 사용할 수 있다.

### 배열 선언

1. int\[ \] x, y; - x는 int타입 배열이다. - y는 int타입 배열이다.
2. int x\[ \], y; - x는 int타입 배열이다. - y는 int타입이다.
3. 참고 : IntArray.java, JButtonArray.java

### 개선된 for문

* **for\(변수명 : 배열명\)** - 변수명 : 객체배열 안에 들어있는 타입 / 변수이름 - 배열명 : Vector와같은 객체배열 이름
* 참조 : [https://java119.tistory.com/107](https://java119.tistory.com/107)
* 참고 : IntArray.java

### 생성자의 역할

* 코드를 작성할때에는 main을 최소화하고 인스턴스화와 호출을 이용해야한다.
* 호출은 생성자를 이용하도록한다.
* 생성자는 해당 클래스에 선언된 변수나 메서드들을 자신의 파라미터를 이용해 사용하기 쉽게 만들어 준다. 
* 참고 : DeptVO.java, JButtonArray.java

### IntArray.java

```java
package ocjp.basic;
//배열의 시작 인덱스는 항상 0이다.[0]
//a2의 주소번지와 a2[0]방의 주소번지가 같기때문이다.
public class IntArray {
	void a2Print(int[] a) {
		
		for(int i=0;i<2;i++) {//배열의 크기=2, 2개만 보여줘
	  //for(int i=0;i<a.length;i++) {//a.length는 참조형이다.
			System.out.println(a[i]);//0 1
		}//end of for
		
		//개선된 for문, 너가 가진거 다 보여줘
		for(int b:a) {//a가 가진거 다 보여줘 a2[10]이라면 10개 값이 나온다.
			System.out.println(b);
		}
	}//end of a2Print

	public static void main(String[] args) {
		int i, j = 0;//j만 초기화 된것.
		i = 2;
		System.out.println(i+", "+j);
		int x[], y = 0;//y=int 라서 오류가 없고
		//int[]a, b = 0;//b=b[] 라서 오류가 생긴다.
		int[]a2, b2;//배열 두개 선언		
		//선언할때에는 반드시 [ ]가 있어야 하지만 생성할때에는 생략한다.
		//파라미터 자리에 배열을 넘길 수 있다. -> 클래스 활용
		a2 = new int[1];//값 2개 0 0
		b2 = new int[3];//값 3개 0 0 0 

		IntArray ia = new IntArray();
		ia.a2Print(a2);//a2[2]는 
	}
}
```

* 13-15번 : a배열이 가진 모든 값을 출력한다.
* 22번 : x는 배열로, y는 int로 선언된 것이다.
* 23번 : a와 b 둘다 배열로 선언된 것이다.
* 24번 : a2와 b2 둘다 배열로 선언된 것이다.
* 27-28번 : int 배열안의 값이 정의되지 않았다면 0이다.

### DeptVO.java

```java
package book.ch5;
//실력에 차이가 나는걸 좋아하지 않는다.
//틀을 짜두고 그 틀에 업무에 대한 코딩만 하게 한다.=framework(spring), 게임엔진 등..

public class DeptVO {//VO = value object
	    //private : 해당클래스에서만 사용가능
		private int    deptno = 0;
		private String dname  = null;//클래스는 선언시 초기값은 null이다.
		private String loc    = null;//=건축허가가 낫지만 아직 건설되지 않은 부지
		
		public DeptVO() {}

		public DeptVO(int deptno, String dname, String loc) {
			this.deptno = deptno;
			this.dname = dname;
			this.loc = loc;
		}
		
		//private 메소드를 public으로 풀어주어서 다른 클래스에서는 메소드로 정해진 값만 사용가능
		public int getDeptno() {
			return deptno;
		}
		public void setDeptno(int deptno) {
			this.deptno = deptno;
		}
		public String getDname() {
			return dname;
		}
		public void setDname(String dname) {
			this.dname = dname;
		}
		public String getLoc() {
			return loc;
		}
		public void setLoc(String loc) {
			this.loc = loc;
		}	
}
```

* 9Days 에서는 하나하나 해당 private메서드를 public메서드로 만들어 주었었지만 생성자를 배운 지금은 저렇게 할 필요가 없다.
* 11번 : 디폴트 생성자이다.
* 13-17번 : 부서번호와 부서명, 지역을 파라미터로 갖는 생성자이다. - 클래스 밑에 생성된 값들을 사용할 수 있게 해준다. - this는 자기자신으로, 여기서는 DeptVO클래스를 가리킨다.

### 배열의 초기화, 생성자 만들고 호출하기

```java
package ocjp.basic;
import javax.swing.JButton;
//상속관계에서 자식이 변수와 메소드 갯수가 더 많다.
import javax.swing.JFrame;

//A is a B ->B가 아빠 : sonata is a car
//할아버지(원본메서드) 아빠(overlodaing된, 개선된 메서드)
public class JButtonArray extends JFrame{
	//10번과 11번의 차이점	
	String labels[] = {"조회", "입력", "삭제"};//선언, 생성, 초기화까지 완료
	JButton jbtns[] = new JButton[3];//선언, 생성만 했다. 초기화는 아직
	
	//생성자 선언해보기
	public JButtonArray(String title) {
		this.setTitle(title);
		initDisplay();//생성자에서의 메서드 호출
	}
	
	public JButtonArray(int height) {
		this.setSize(700, height);
		initDisplay();
	}

	public void initDisplay() {
		this.setSize(400, 300);//나는 어디에 선언된 메서드인가요? 아빠를 찾아주세요 = JFrame, this=나=JButtonArray
		this.setVisible(true);//메서드 호출시 파라미터의 갯수가 같나요?
	}	

	public static void main(String[] args) {
		new JButtonArray("생성자에 대해서..");
		new JButtonArray(100);
	}
}
```

* 10-11번 : 생성과 초기화의 차이
* 14번 : String타입의 파라미터를 화면의 title로 초기화해주는 생성자
* 19번 : int타입의 파라미터를 화면의 높이로 초기화해주는 생성자

### GitHub Repositories 만들기

1. git hub 가
2. 로컬 PC에 공유할 폴더 생성
3. Git 다운로드 - 도스 : git --version
4. 공유 폴더안에 숨겨진 git파일 만들기 - 공유 폴더 우클릭 -&gt; git bash -&gt; git init
5. git hub에서 repositories 생성

후기 : 휴일이 다가와서 늘어지는게 아닌가 하는 생각이 든다 ㅠㅠ 힘내서 끝까지!

