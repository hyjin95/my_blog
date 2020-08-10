---
description: 2020.08.10 - 9일차
---

# 9 Days - Switch문

### Switch-case 

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
	
		//static메소드인 메인안에서 static으로 선언된 switch메소드를 호출시, 클래스이름.메소드이름으로 호출한다.
		int out = SwitchTest.switchit(x);
		System.out.println("j+x = "+out);	
	}

}

```

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

