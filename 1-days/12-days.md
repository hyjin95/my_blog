---
description: 2020.18.13 - 12일차
---

# 12 Days - 생성자, 객체 배열, API, 클래스, 인스턴스화 정리하기

생성자의 역할, 활용, 객체배열 초기화, api보는법, 추상클래스와 인터페이스, 인스턴스화, 생성자와 인스턴스화, 상속관계, 디폴트 생성, overloading, overriding, 콜백메서

### GitHub Repositories만들기

### 배열 선언하기, 개선된 for \(변수명:배열명\)

[https://java119.tistory.com/107](https://java119.tistory.com/107)

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

### 생성자 이해하기

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
	
	//디폴트 생성자 선언해보기
	//디폴트 생성자의 역할 : 
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
		new JButtonArray("생성자에 대해서..");//파라미터가 없는 생성자를 호출하는 문장,
		new JButtonArray(100);
	}
}


```

