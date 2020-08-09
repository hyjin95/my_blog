---
description: 2020.08.06 - 7일차
---

# 7 Days - JMenuBar, Event

```java
package book.ch4;

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

