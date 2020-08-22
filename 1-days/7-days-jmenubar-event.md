---
description: 2020.08.06 - 7일차
---

# 7 Days - JMenuBar, Event, get, set

### 6일차 복습

* 변수 - 변수가 몇개 필요한지 말할 수 있다. - 변수의 위치를 정하고 초기화 할 수 있다. - 메소드 호출시 변수들을 파라미터로 초기화 할 수있다.
* 메소드 - 메소드 선언 순서를 알고 있다.   \(제한자, 타입, 메소드이름, \(파라미터\), {}\) - 리턴타입, 파라미터에 원시형 타입을 사용할 수 있다.  - Object, 객체를 알고있다.

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261 : Oracle.com
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5

### Event

* 참고 : 8 Day - BaseBallGame.java\(이하 BBG\)

1. **Action Listner 인터페이스**를 클래스에 장착한다.=**이벤트 감지 -** 이벤트 처리 인터페이스가 다른 이벤트 소스도 있다. - 이벤트 소스 : 버튼,체크박스 등 이벤트가 일어나는 주소번지 - BBG에서는 여러 이벤트 소스를 처리하기 위해 메소드를 만들도록 한다. - **class 클래스이름 implements Action Listner{ }** - implements : 인터페이스 예약어 
2. 이벤트를 처리할수 있는 메소드를 재정의 한다. - **public void actionPerformed \(ActionEvent e\){ }** - actionPerformed : callback 메소드 - callback 메소드 : 메인메소드안에서 개발자가 호출하지 않아도 JDK가 감지하면 자동으로 호출되는 메소드를 말한다. 예시로 main메소드도 개발자가 호출하여 실행하지 않고 시스템이 자동으로 호출해주는 callback메소드이다. - 이벤트 처리 기능을 메소드로 꺼내어 재사용성을 높여야한다.

* **이벤트 핸들러** 클래스 A - 이벤트 처리를 장착한 클래스를 부르는 말이다. - 어떤 클래스의 뒤에 Action Listner이 있으면 그 클래스 안에서 이벤트 처리를 하겠다는 의미를 갖는다
* 아자스 - 검색창의 자동완성기능과 같이 페이지의 새로고침이나 사용자의 요청없이도 반응해 주는것을 말한다.
* 이벤트를 느끼는 주체 : sw, 자바가상머신 - 버튼을 누르는 것을 감지하는것은 개발자가 아니라 자바가상머신이다.
* 이벤트가 어떤 인터페이스와 맞는지 찾아서 해당 인터페이스에 implements 해야한다.
* callback메소드도 이벤트에 따라 다르다.
* 이벤트에서 if를 사용하면 여러 이벤트를 대응시킬 수 있다. - if,if구문은 조건을 계속 따져야하기 때문에 시스템이 처리할 일이 많다. - if, else-if는 조건을 만족하면 넘어가기때문에 시스템이 처리할 일이 적다.

### getter메서드와 setter

* getter - read의 역할로 리턴 값을 반환한다. 사용자가 담은 것을 꺼낸다. - get메소드는 꺼낸 값을 반환하므로 리턴타입을 갖는다. - 꺼내오는 역할로 파라미터는 필요없다. - print로 메소드호출이 가능하다. print\(get메소드\);
* setter - set메소드는 리턴타입을 갖지않는다.\(==void\) - write, save의 역할로 목적을 갖지 않는다. 사용자가 담은 것을 담는다. - 값, 파라미터가 필요하다.
* 참고 : 15Days - NickNameList.java

### random class

* import 추가 한다.
* Random r = new Random\(\); - r.nextInt\(int\):int-Random - r : 소유주의 개념으로 인스턴스변수나 클래스가 올 수 있다. - Integer.parseInt\(\)는 클래스의 개념으로 r의 자리에 올 수 있다.
*  Integer.parseInt = static int parseInt\(string\) - int : 타입 - parseInt\(string\) : 메소드
* Static 안에서 클래스 호출 시에는 인스턴스화를 해야하지만 그 외 메소드에서 호출은 인스턴스화 없이 첫문자를 대문자로 써서 호출한다. **I**nteger.parseInt

### 사용자로부터 입력 값을 읽어오는 클래

* BufferedReader br = new BufferedReader\(new.InputStreamReader \(system.in\)\);
* Scanner 클래스

### 변수를 지정하는 이유

* 키보드\(입력\) -&gt; 읽기 -&gt; 처리 -&gt; 모니터\(출력\)
* 처리 = 변수호출 - 저장된 변수기억 불러오기 - **변수 하나로 여러 값을 기억** 시킬수 있다. - ex\) mycar.color; mycar.wheelnum; ...

### IO 패키지

* IO == input, output
* 읽고 쓰는, 카메라 필터와 같이 효과만 줄수 있는 역할을 하는 클래스들의 패키지
* read\( \) : 한 글자만 읽어온다. readline\( \) : 한 문장을 읽어온다. 

### toad 에서 날짜표시해보기

* SELECT sysdate FROM dval - sysdate +1 : 다음날, 내일  - sysdate -1 : 전날, 어제

### 다른 주소를 갖는 같은 클래스의 인스턴스 변수

```java

public class StringTest {

	public static void main(String[] args) {
		String s1 = new String("apple");
		String s2 = new String("apple");
		String s3 = "haha";
		//s1의 주소번지와 s2의 주소번지가 같은가? 다르다
		System.out.println(s1==s2);/////////false 출력
		//s1이 가리키는 문자열과 s2가 가리키는 문자열이 같은가? 같다
		System.out.println(s1.equals(s2));//true 출력
	
	}

}

```

###  Math.random을 이용한 번호뽑기

```java
package book.ch4;

public class RandomTest {

	public static void main(String[] args) {
		int com[] = new int[3];
		//Math.random클래스 사용. 실수범위라서 *10을 해준다. 범위는 0.0에서 1.0
		
		for(int n=0;n<=9;n++) {//임의의 숫자로 방 세개를 채우는것을 10번 반복한다.
		
		com[0] = (int)(Math.random()*10);
		
		do {
			com[1] = (int)(Math.random()*10);
		}while(com[0]==com[1]);//0번 방과 1번 방 값이 같으면 다시 반복한다.
		
		do {
			com[2] = (int)(Math.random()*10);
		}while(com[0]==com[2] || com[1]==com[2]);
		
		//""를 넣는이유 = 덧셈방지
		System.out.println(com[0]+""+com[1]+""+com[2]);
	  
	  }//end of for
	}
}

```

### 시스템이 랜덤 채번한 수를 사용자가 맞추는 프로그램

```java
package report.ch4;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.Random;

public class RandomGame {

	public static void main(String[] args)
	throws IOException//예외처리를 직접하지 않고 미룬다.
	
	{		
		Random r = new Random();
		int dap = r.nextInt(10);//dap=채권한 숫자
		
		BufferedReader br = 
				new BufferedReader(
						new InputStreamReader(System.in));
		System.out.println("0부터 9사이의 숫자를 하나 입력하세요.");//입력되는 숫자는 문자타입
		String str = null;//값이 정해지지 않았다.
		
		while((str=br.readLine())!=null) {//뭐든 입력만 하면 파일이 끝날때 까지 읽는다.
			System.out.println("사용자가 입력한 값 = "+str);
			
			if(Integer.parseInt(str)==dap) {//입력과 채권 수가 같으면 종료
				System.out.println("정답입니다.");
				break;
			}//end of if 
			
			else if(Integer.parseInt(str) > dap) {
				System.out.println("Down");
			}//end of else if
			
			else if(Integer.parseInt(str) < dap) {
				System.out.println("UP");
			}//end of else if
			
			if("q".equals(str)) {
				System.out.println("프로그램을 종료합니다.");
				break;
			}//end of if
		}//end of while
		
	}

}

//boolean isStop = false;
//int i = 1;
//while(!isStop) {//true입력시 오류
//	int num = r.nextInt(10);//0-9랜덤뽑기
//	System.out.println(num);
//	
//	if(i>5) {
//		break;
//	}//end of if
//	i++;
//}//end of while
```

### 6일차 숙제의 여러방법

```java
package book.ch4;

import javax.swing.JOptionPane;

public class Test08052 {
	//피보나치 수열의 규칙을 찾아서 a1, a2, a3, ..., a10항까지 출력해주는 프로그램을 작성하시.(for, if 사)
	
	void methodA( ) {//for
		int i;
		int num1, num2, sum;
		num1 = 0;
		num2 = 1;
		sum = 1;
		
		for(i=0;i<10;i++) {			
			
			System.out.println("a"+(i+1)+"="+sum);//1,1,2,3,5
			sum = num1 + num2;//1,2,3,5
			num1 = num2;//num1=1,1,2,3
			num2 = sum;//num2=1,2,3,5
						
		}//end of for
	}//methodA

	void methodB() {
		int num[] = new int[3];
		num[0] = 1;
		num[1] = 1;
		System.out.println("a0="+num[0]);
		System.out.println("a1="+num[1]);
			
		for(int i=1;i<10;i++) {
			num[2]=num[1]+num[0];
			System.out.println("a"+(i+1)+"="+num[2]);
					
			for(int s=0;s<2;s++) {
				num[s]=num[s+1];
			}//end of for
					
		}//end of for				

	}//end of methodB
	
	void methodC() {
		
		int a = 0;
		int b = 0;
		int sum = 0;
		
		for(int i=1;i<11;i++) {
			if(i<=2) {
				a=1;
				b=1;
				sum=1;
				System.out.println("a"+i+"항은 "+sum);
			}//end of if
			
			else {
				sum=a+b;
				a=b;
				b=sum;
				System.out.println("a"+i+"항은 "+sum);				
			}//end of else
		}//end of for		
		
	}//end of methodC
	
	void methodT() {
		int a1=1, a2=1, a3=0;
		System.out.println(a1+""+ a2+"");
		for (int x=0;x<8;x++) {
			JOptionPane.showInputDialog(null,"before a1:"+a1+",a2:"+a2);
			a3=a1+a2;
			System.out.println(a3+"");
			a1 = a2;
			a2 = a3;
			JOptionPane.showInputDialog(null,"after a1:"+a1+",a2:"+a2);
		}
	}	
	
	public static void main(String[] args) {
		Test08052 t08052 = new Test08052();
		
		//t08052.methodA();
		//t08052.methodB();
		//t08052.methodC();
		t08052.methodT();		
		
	}

}
```

### 숙제

시스템이 10에서 -10사이의 정수 10개를 랜덤하게 채번하여 음수와 양수의 합계를 구하는 프로그램을 작성하시오

```java
package report.ch4;

import java.util.Random;

public class Report0806 {
	
	void methodA( ) {
		
		Random r = new Random();
		
		int i;
		int result;
		int minus=0;
		int plus=0;
		
		for(i=1;i<=10;i++) {
		
			int num = r.nextInt(10);
			
			if((result = r.nextInt(2))==1) {
				num = num*-1;
				System.out.println(num);
				minus = minus+num;
			}
			
			else {
				System.out.println(num);
				plus = plus+num;
			}								
			
		}//end of for	
		
		System.out.println("음수의 합 = "+minus);
		System.out.println("양수의 합 = "+plus);
		
	}

	public static void main(String[] args) {
		Report0806 r0806 = new Report0806();
		r0806.methodA();			

	}

}

```

