---
description: 2020.08.10 - 9일차
---

# 9 Days - Switch문, JAVA로 Oracle이해하기

### 복습

연관되는 단어들을 모아보자

1. boolean - for문 : for\(초기화,조건식,증감연산\) : 반복, if문 : if\(조건식\) : 판단, while문, do-while문, 실행문, return값 : true or false, 논리형, 변수, &&, \|\|
2. int\[ \] - 배열, 하나의 타입, 여러개의 값, 정수, for문, 파라미터, 인덱스 값, 랜덤, 주소번지, 방의 시작 값 : 0, 비교
3. class - 메소드, 호출, import, 객체, 부모 자식, 인스턴스화, 클래스선언, 전역변수, 초기화 생략, Heap, 내 안의 메소드끼리 호출가능, static 메소드는 다른 메소드 호출시 인스턴스화 해야 호출가능

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261 : Oracle.com
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5

### 객체 지향

* **OOP** - 객체지향, 상하관계, 수직적 관계 - 상속의 개념 \(추상클래스 &lt;--&gt; 인터페이스\) : 정의만 되어있는, 미지정된 - 게임엔진에서 많이 사용된다. - 대표적 언어 : java
* **AOP** - 관점지향, 수평적 관계 - 대표적 언어 : C

### Overriding, Overloading

* 변수 = 데이터, 메소드 = 처리
* 변수 이름은 중복될 수 없다.
* 메소드는 같은 이름의 메소드가 두 개 이상 올 수 있다. \(main제외\)
* overriding : 상속관계의 경우일때 사용한다.
* overloading : 상속관계를 따지지 않는다. 필요조건이 아니다.

### java와 Toad for Oracle

java와 Oracle을 같이 생각할 줄 알아야 한다.

| **java** \(언어\) | **Oracle** \(Data관리\) |
| :---: | :---: |
| string | Varchar2 |
| int | number\(4\) |
| double | number\(7, 2\) |
| time stamp | date |

* **Varchar2 : 가변형 타입** - oracle에서 문자를 담는 타입으로 java의 string과 같다.
*  oracle문자 타입은 char, Varchar 두 종류가 있다.
*  8bit = 1byte\(8칸\) 에 "hello"라는 5글자를 넣을때, -  char은 남는 3칸에 띄어쓰기를 넣어 빈자리를 메꾼다. : 공간관리가 비효율적이다, 고정형이다. - Varchar은 남은 3칸을 반환시킨다. : 공간관리가 효율적이다, 가변적이다.
* Varchar2에서 반납한 3칸을 수정하고 싶을때 수정 값이 기존의 5칸을 넘어가면 여분으로 남겨놓은 bit에 넣거나 bit 칸에 주소를 넣는다. : 주소를 타고 두개의 byte를 읽어야 하므로 읽기 속도가 느려진다.
* 영문은 한 자에 1bit, 한글은 2bit를 차지한다.
* **number\(4\)** : 정수타입, 9,999 까지 담을 수 있다. 값이 넘어가면 stackoverflow된다.
* **number\(7,2\)** : 실수타입, 9,999.99 까지 담는다.
* Time stamp는 잘 사용하지 않고 보통 String으로 받아서 형전환 한다.

### Toad Data 읽기

* 명령어 선택 -&gt; f4 -&gt; data
* 행\(가로, 로우\) : '내' 값, 변수 이름
* 열\(세로\)          :  해당 변수의 값들, 같은 타입의 값들

### Switch-case 이해

```java
package report.ch4;

public class SwitchTest {
	
	public int others(int x) {
		//switch(x%35) 가능:35로 나눈 '값'
		switch(x) {//x=5이므로 case4에서 시작
		   case 6 : x--;
		   case 5 : x--;//5 -> 4
		   case 4 : x--;//4 -> 3
		   case 3 : x--;//3 -> 2
		   break;// switch 탈출
		   default : x--;
		   break;
		}//end of switch
		return x;
	}
	//메소드 앞에 static이 있으면 인스턴스화가 필요없다.
	//static이 없는 메소드를 호출 할 때에는 무조건 인스턴스화 해야한다.
	//파라미터 자리는 지역변수 자리이다. = 초기화 필수
	//선언만 하는것은 문제되지 않지만, 사용할때 문제가 된다.
	public static int switchit(int x) {//파라미터의  x값은 호출할때 결정된다.
		int j = 1;
		switch(x) {//x=5이므로 case 5에서 시작
		   case 1 : j++;
		   case 2 : j++;
		   case 3 : j++;
		   case 4 : j++;
		   case 5 : j++;//1 -> 2
		   default : j++;//2 -> 3 default=else
		}//end of switch
		return j+x;//3+5 = 8
	}
	
	public static void main(String[] args) {
		int x = 5;
		SwitchTest st = new SwitchTest();
		System.out.println("x = " +st.others(x));
	
		//static메소드인 메인안에서 static으로 선언된 switch메소드를 호출시, 
		//클래스이름.메소드이름으로 호출한다.
		int out = SwitchTest.switchit(x);
		System.out.println("j+x = "+out);	
	}
}
```

* switch의 x값에 해당하는 case부터 진행된다.
* break; 가 없으면 계속 진행되므로 주의해야 한다.
* switch문의 default는 if문의 else와 같은 역할을 한다.
* 
### switch-case에 조건 넣어보기

참고 : 5 Days- 숙제

```java
package book.ch4;

public class FizzBuzzGame5 {//메소드 조각내기
	
	public void methodA(int start, int end) {//범위를 수정지정할 수 잇다.
		
		int i=1;
		
		while(i<end) {
			switch(i%35) {//큰 조건				
				case 0 : {//i를 35로 나눈 값이 0이니?
					System.out.println("fizzbuzz");
					break;
				}							
				case 5: case 10: case 15: case 20: case 25: case 30: {
						System.out.println("fizz");
						break;
					}//end of else if
				
				case 7: case 14: case 21: case 28: {
					System.out.println("buzz");
					break;
				}//end of else
				default: {
					System.out.println(i);
				}//end of default
			 }//end of switch
			i++;
		}//end of while
	}//end of methodA

	public static void main(String[] args) {
		
		FizzBuzzGame5 fbg = new FizzBuzzGame5();
		fbg.methodA(1,100);		

	}//end of main
}//end of class
```

* 5일차의 숙제였던 FizzBuzzGame을 switch문으로 전환 시켜본다.\
* switch문의 \(파라미터\)에는 값이 들어간다. i%35는 결과 값이 값이므로 들어갈 수 있다.
* if문과 마찬가지로 가장 범위가 큰 조건 부터 정의한다. 5의 배수가 case 30까지만 있는 이유는 그 이후로 40은 35로 나누면 5이기 때문에 case 5에 해당되기 때문이다. 7의 배수도 마찬가지로 28까지만 표시한다.
* 위의 경우처럼 비교해야하는 값이 많은 경우에는 switch문을 사용하기에 적절하지 않다.

### switch-case 와 for

```java
package report.ch4;

public class SwitchTest2 {
	String msg;
	public void methodA( ) {
		int i = 0;
		boolean isOk = false;
		while(!isOk) {
			String msg = "오늘 스터디 할까?";
			switch(i) {
			case 0: {
				String msg = "";////위에서 변수타입을 선언했으므로 타입이 또 있으면 오류가 생긴다. 		
			}break;
		    default:
			break;
     	  }//end of switch
		}//end of while
	}//end of methodA

	public static void main(String[] args) {
		int j = 0;
		for(int i=0;i<3;i++) {			
		}
		for(j=0;j<3;j++) {			
		}		
		System.out.println(i);//for문 안에서 i를 선언한 경우 for문밖에서 인식할 수 없다.
		System.out.println(j);
	}//end of main
}

```

* 12 번은 타입선언이 중복되어 오류가 발생한다.
* main메소드에서 i는 for문 안에서 선언되어 for문 밖에서는 유지되지 않는다. 26번은 오류가 발생한다.

### Oracle의 data를 java로 이해하기

```java
package book.ch5;

public class DeptVOSimulation {

	public static void main(String[] args) {
		//부서번호 10, 부서이름 accounting, 부서위치 newyork
		DeptVO dVO = new DeptVO();//한번에 하나의 로우를 담는다.
		dVO.setDeptno(10);
		dVO.setDname("Accounting");
		dVO.setLoc("NEW YORK");
		System.out.println(dVO.getDeptno());
		System.out.println(dVO.getDname());
		System.out.println(dVO.getLoc());
		
		dVO = new DeptVO();//변수이름은 같아도 주소는 다르다. 복사, 이사 : 한번에 하나의 로우를 담는다.
		dVO.setDeptno(20);
		dVO.setDname("RESEARCH");		
		dVO.setLoc("DALLAS");
		System.out.println("===============");
		System.out.println(dVO.getDeptno());
		System.out.println(dVO.getDname());
		System.out.println(dVO.getLoc());
		
		dVO = new DeptVO();//주소 복사, 이사 : 한번에 하나의 로우를 담는다.
		dVO.setDeptno(30);
		dVO.setDname("SALES");		
		dVO.setLoc("CHICAGO");
		System.out.println("===============");
		System.out.println(dVO.getDeptno());
		System.out.println(dVO.getDname());
		System.out.println(dVO.getLoc());
	}//위의 부서번호,이름, 위치는 고정 값이다. 
	//값이 수정되었을때 죽은 값이 나오는 오류가 날 수 있어서 이런방법을 선호하지 않는다.
	//부서가 여러 개인 경우에 이런방법은 쓸 수 없다.

}//Eclipse에서 debug모드로 변화를 확인할 수 있다.
```

* class안에는 Oracle에 관리되는 정보들을 담을 수 있다.
* n개의 타입, n개의 값을 담을 수 있지만 한 번에 하나의 값만 볼 수 있다.
* DeptVOSimulation 클래스에서 인스턴스화를 3번 한 이유이다. = 한번에 한 행\(로우\)만 읽어올 수 있다.

```java
package book.ch5;
//실력에 차이가 나는걸 좋아하지 않는다.
//틀을 짜두고 그 틀에 업무에 대한 코딩만 하게 한다.=framework(spring), 게임엔진 등..

public class DeptVO {//VO = value object
	    //private : 해당클래스에서만 사용가능
		private int    deptno = 0;
		private String dname  = null;//클래스는 선언시 초기값은 null이다.
		private String loc    = null;//=건축허가가 낫지만 아직 건설되지 않은 부지
		
		//메소드를 public으로 풀어주어서 다른 클래스에서는 메소드로 정해진 값만 사용가능
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

* private으로 해당 클래스에서만 사용할 수 있게 묶어둔 클래스들은 public 메소드로 호출되어야만 다른 클래스에서 접근이 가능하다.
* 한번에 메소드로 푸는 법 class드래그 -&gt; 우클릭 -&gt; source -&gt; Generate Gatter and Setters

후기 : 기록들을 기록만 하는데에 그치지 않고 수시로 들러서 보기 좋게 수정하고, 추가 내용을 달면서 복습하도록 해야한다. 노력하자!

