---
description: 2020.08.05 - 6일차
---

# 6 Days - 반복문과 배열

### 학습목표

* while, for, do{}while문을 맞게 사용할 수 있다.

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261 : Oracle.com
* 사용Tool  Eclipse : Eclipse.org Oracle Database 11g Toad DBA Suite for Oracle 11.5

### Oracle과 자바

| men\_id\(string\) | men\_pw\(string\) | men\_name\(string\) |
| :---: | :---: | :---: |
| test | 1234 | ○○○ |
| apple | abcd | △△△ |
| ... | ... | ... |

* **SELECT 컬럼명1, 컬럼명2** SELECT men\_name -&gt; men\_name은 리턴 값 이다. 위의 표에서는 ○○○과 같은 이름을 말한다.
* **FROM 집합이름**
* **WHERE 컬럼명=:x** 컬럼명=집합에 있는 컬럼명, x=값 WHERE men\_id=:id AND men\_pw=:pw 로그인 시 id와 pw둘다 필요하므로 and가 있어야 하고, 여기서 오른쪽항에 사용자가 입력하는 id와 pw는 변수이자 파라미터가된다.\(string type\) id와 pw는 사람마다 다른 값을 가지고 있고, 다른 사용자의 값도 받아줘야 하기 때문에 초기화를 해야한다. = 메소드안에 선언하는 지역변수이다.

### 반복문 

* for : for\(초기화, 조건, 증감연산\) {실행문}
* while : while\(조건 {실행문, 증감연산}
* do-while : do{실행문,증감연산}while\(조건식\);
* \(조건\)의 위치 for과 while은 실행문이 조건문 뒤에있어서 조건문을 먼저 따진다. 조건을 먼저 따지기 때문에 조건에 부합되지 않으면 실행이 한번도 되지 않을 수 있다. do-while은 실행문의 조건문 앞에 있기때문에 조건을 따지기 전에 실행되므로 무조건 **한번은 실행**된다.
* **for**  - for문 안에서 초기화 할때 타입을 선언하면 그 변수는 for문 밖에서는 사용될 수 없다. 주의하자.
*  **while**  - 순서도에 가장 부합하는 반복문  조건식, 초기화, 증감연산을 따로 넣어 순서도의 진행 가장 비슷하게 작성된다.
* **do-while**  - 첫 조건을 따지지 않는 코드에서 많이 사용한다. 예를 들어 컴퓨터가 지정한 임의의 수 3개를 사용자가 3번의 기회 동안 맞추는 게임 프로그램이라면,  일단 컴퓨터가 숫자를 한개씩 뽑는다. 이때 비교할 숫자가 없으므로 첫번째 뽑기에는 조건문이 필요하지 않다. - while문 뒤에 증감연산자가 없으면 다음으로 넘어가지 않기 때문에 무한히 반복된다.\(**무한루프**\) 그러므로 반드시 막아주는 증감연산자가 필요하다.

### for문과 while문으로 결과값 같은 코드를 써보자

```java
package book.ch4;

import java.util.Scanner;

//모든 클래스는 Object클래스를 상속 받고 있다.
//그래서 Object에 정의된 메소드는 모두 내가 사용할 수 있는것이다.

public class ScannerTest {

	public static void main(String[] args) {
		
		int i = 0;
		//JoptionPane.showIputDialog역할 = scanner 역할
		//scanner 클래스는 util 밑에 있어서 import해야 사용가능한 예약어이다.
		Scanner scan = new Scanner(System.in);
		
		for(i=1;i<4;i++) {//i=1에서 i++되는데 i가 4미만이면 실행한다.=4번
			System.out.println("숫자를 입력하시오.");
			String num = scan.nextLine();
			System.out.println("num = "+num);
			//i=4
		}//end of for
		
		//System.out.println("i="+i);
		
		i = 1;//for문에서 사용된 i값 초기화
		while(i<4) { //4번 실행문을 반복한다.
			System.out.println("숫자를 입력하시오.");
			String num = scan.nextLine();
			System.out.println("num = "+num);
			i++;//이 단항증감식이 없다면 while문은 무한반복한다.
			//i=4
		}//end of while
		
	}

}

```

* 사용된 클래스 : **Scanner** 클래스 JOptionPane.showInputDialog와 비슷한 역할을 한다. showInputDialog는 직접 입력창을 띄워주고, Scanner는 다음라인에 입력된 값을 가져온다. import해야한다. \(java.util.Scanner\)
* 
### 배열

* 배열의 쓰임새  - 변수는 하나의 타입, 하나의 값만 담을 수 있다. 배열은 하나의 타입, **여러개의 값**을 담을 수 있다.
* int is \[ \] = new int \[ 3 \]; ==int 타입의 is라는 이름의 배열은 int 타입인 방을 3개 갖는다. 첫번째 방의 기본값은 0이다.

### 배열을 이해해보자

```java
package book.ch4;

public class ArrayTest {

	public static void main(String[] args) {
		int is[] = new int[3];//한번에방 3개 생성
		is[0] = 1;//0번 방 1으로 초기화
		is[1] = 2;//1번 방 2으로 초기화
		is[2] = 3;//3번 방 3으로 초기화
		
		double ds[] = new double[1];//방 1개생성

		//j가 0이고 ++되는데 3이하면 is[j] 방의 값을 출력한다.
		for(int j=0;j<3;j++) {
			System.out.println(is[j]);
			
		}//end of for
	}

}

```

* 출력 결과 1 2 3

### 배열과 do-while을 이용해 코드 작성하기

```java
package book.ch4;

public class RandomTest {

	public static void main(String[] args) {
		int com[] = new int[3];
		//Math.random클래스 사용. 실수범위라서 *10을 해준다. 
		//범위는 0.0에서 1.0
		
		for(int n=0;n<=9;n++) {//임의의 숫자로 방 세개를 채우는것 10번 반복
		
		com[0] = (int)(Math.random()*10);
		
		do {
			com[1] = (int)(Math.random()*10);
		}while(com[0]==com[1]);//0번 방과 1번 방 값이 같으면 다시 반복
		
		do {
			com[2] = (int)(Math.random()*10);
		}while(com[0]==com[2] || com[1]==com[2]);
		
		//""를 넣는이유 = 덧셈방지
		System.out.println(com[0]+""+com[1]+""+com[2]);
	  
	  }//end of for

	}

}

```

* 사용 클래스 : Math.random  Math.random 클래스는 패키지에 포함되어있어 import선언이 필요없다. Math가 제공하는 함수 중에 random 함수는 0.0~1.0사이의 숫자 값을 반환한다.
* 위 코드에서는 int타입으로 출력하기 위해 \*10을 하였다.
* 결과값으로 컴퓨터가 임의로 뽑은 중복되지않는 3개의 숫자 묶음이 10종류 출력된다.

### 예제 문제

컴퓨터가 1~10 중 임의의 숫자를 뽑고, 사용자가 숫자를 맞추는 코드를 작성하고 메소드를 쪼개보시오.

```java
package book.ch4;

import java.util.Random;//확인하기
import javax.swing.JOptionPane;//확인하기

public class Test_random {
	
	int random(int num) { //0~9사이의 임의의 숫자를 뽑아주는 메소드
		                
        Random tr = new Random();        
      
        num= tr.nextInt(10);         
        
        return 0;
	}
	
		
	public static void main(String[] args) {
		
		Test_random tr = new Test_random();
		
		int num1;
		String input;
		
		input = JOptionPane.showInputDialog("숫자를 맞춰보세요.");
	    int num2 = Integer.parseInt(input);
	    
	    num1 = tr.random(10);
	    
	    if(num2==num1){
        	System.out.println("정답입니다.");
        }
        else {
        	System.out.println("틀렸습니다.");
        }
        System.out.println(num1);
        System.out.println(num2);          

	}

}

```

### 5일차 숙제

#### for문 메소드로 조각내기

```java
package book.ch4;

public class Test0805_method_T {//메소드 조각내기
	
	public void methodA() {
		
		for(int i=1; i<=100; i=i+1) {
			
			if((i%5==0)&&(i%7==0)) {
				System.out.println("fizzbuzz");
			}//end of if
			
			else if(i%5==0) {
				System.out.println("fizz");
			}//end of else if
			
			else if(i%7==0) {
					System.out.println("buzz");
				}//end of else if
			
			else {
				System.out.println(+i);
			}//end of else
		}//end of for
					
	}//end of methodA

	public static void main(String[] args) {
		
		Test0805_method_T tmt = new Test0805_method_T();
				tmt.methodA();		

	}//end of main

}//end of Test0805_method_T


```

#### 보고싶은 결과의 범위를 지정할 수 있게 하기\(파라미터 이용\)

```java
package book.ch4;

public class Test0805_Parameter_T {//메소드 조각내기
	
	public void methodA(int start, int end) {//범위를 수정지정할 수 잇다.
		
		for(int i=start; i<=end; i=i+1) {
		//for(int i=1; i<=100; i=i+1) {
			
			if((i%5==0)&&(i%7==0)) {
				System.out.println("fizzbuzz");
			}//end of if
			
			else if(i%5==0) {
				System.out.println("fizz");
			}//end of else if
			
			else if(i%7==0) {
					System.out.println("buzz");
				}//end of else if
			
			else {
				System.out.println(+i);
			}//end of else
		}//end of for
		//i=11
					
	}//end of methodA

	public static void main(String[] args) {
		
		Test0805_Parameter_T trt = new Test0805_Parameter_T();
				trt.methodA(5,10);		

	}//end of main

}//end of Test0805_method_T


```

#### for문 while문으로 바꿔보기

```java
package book.ch4;

public class Test0805_While_T {//메소드 조각내기
	
	public void methodA(int start, int end) {//범위를 수정지정할 수 잇다.
		
		int i=1;
		
		while(i<end) {
		
			if((i%5==0)&&(i%7==0)) {
				System.out.println("fizzbuzz");
			}//end of if
			
			else if(i%5==0) {
				System.out.println("fizz");
			}//end of else if
			
			else if(i%7==0) {
					System.out.println("buzz");
				}//end of else if
			
			else {
				System.out.println(+i);
			}//end of else
			i++;
		}//end of while
	}//end of methodA

	public static void main(String[] args) {
		
		Test0805_While_T twt = new Test0805_While_T();
		twt.methodA(1,100);		

	}//end of main

}//end of Test0805_method_T

```

### 예제 문제 2

#### 오늘 배운 내용으로 구구단 프로그램 작성해보

```java
package book.ch4;

import javax.swing.JOptionPane;

public class Gugudan {
	
	int i;
	int j;
	
	void methodA(int input2) {//구구단 for 메소드			
	
		//for(i=input2;i<=input2;i++) {//단 증가 반복문
			
			System.out.println(input2+"단");
			
			for(j=1;j<10;j++) {//곱하기 증가 반복문				
				System.out.println(input2+"*"+j+"="+(input2*j));
			}//end of for			
		//}//end of for		
						
	}//end of methodA
	
	void methodB( ) {//구구단 while 메소드		
		i = 1;
		
		while(i<10) {
			
			System.out.println(i+"단");			
			j = 1;//j값 초기화
			
			while(j<10) {
				
				System.out.println(i+"*"+j+"="+(i*j));
				j++;				
			}//end of while			
			i++;
		}//end of while		
	}

	public static void main(String[] args) {
		
		 String input = JOptionPane.showInputDialog("몇단을 불러올까요?");
	     int input2 = Integer.parseInt(input);
	     
		 Gugudan ggd = new Gugudan();		 
		 ggd.methodA(input2);
		// ggd.methodB();
			

	}

}

```

* 원하는 구구단 부분을 불러올 수 있게 작성했다.

### 자바에서의 event 맛보기

#### ActionListener 인터페이스

```java
package game.baseball;

import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JPanel;
import javax.swing.JScrollPane;
import javax.swing.JTextArea;
import javax.swing.JTextField;

import java.awt.BorderLayout;
import java.awt.Color;
import java.awt.Font;
import java.awt.GridLayout;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
/*
 * 이벤트 처리 3단계
 * 1.JtextField가 지원하는 이벤트 처리 담당 클래스를 implements한다.
 * 2.1번에서 추가된 이벤트 처리 담당 클래스가 선언하고 있는 actionPerformed메소드를 재정의 해야한다.
 * 3.이벤트 소스와(이벤트 처리 대상 클래스의 주소번지)와 이벤트 처리를 담당하는 클래스를 연결하기.
 */

public class BaseBallGame implements ActionListener{//5장
	
	//중앙에 들어갈 속지 선언, 전변
	JPanel jp_center = new JPanel();
	
	//세자리 숫자를 입력후 enter를 쳤을때 사용자가 입력한 숫자와 숫자를 맞추기 위한 힌트문을 출력해줄 화면
	JTextArea jta_display = new JTextArea();
	JTextField jtf_user = new JTextField();
	JScrollPane jsp_display = new JScrollPane(jta_display //스크롤속지위에 TextArea올리기
                    			, JScrollPane.VERTICAL_SCROLLBAR_AS_NEEDED//세로스크롤바는 필요한 상황에 생성하기
			                    , JScrollPane.HORIZONTAL_SCROLLBAR_NEVER); //가로스크롤 생성하지 않기
	
	//글꼴과 글꼴에 대한 스타일과 글자 크기를 인스턴스화를 통해서 정해준다.
	//그 값들은 생성자의 파라미터로 결정된다.(글꼴,스타일,크기)
	Font f = new Font("Thoma",Font.BOLD,14);
	
	//동쪽에 들어갈 속지 생성
	JPanel jp_east = new JPanel();//버튼을 넣으면 width가 늘어난다.
	//east속지에 버튼 추가하기.(새게임, 정답, 지우기, 나가기)
	JButton jbtn_new = new JButton("새게임");
	JButton jbtn_dap = new JButton("정답");
	JButton jbtn_clear = new JButton("지우기");
	JButton jbtn_exit = new JButton("나가기");

	//화면을 그려주는 메소드 선언
	public void initDisplay( ) {
		System.out.println("initDisplay 호출 성공");
		//이벤트 소스와 이벤트 처리 클래스를 매핑하는 코드 추가
		jtf_user.addActionListener(this);//여기서 this는 자기 자신 클래스를 가르킨다.=BaseBallGame:내안 어디엔가에 actionPerformed가 있어야 한다.
		
		JFrame jf = new JFrame();
		
		jta_display.setFont(f);
		
		jp_center.setBackground(Color.black);
		jp_east.setBackground(Color.gray);
		jta_display.setBackground(new Color(255,220,239));//배경색
		jta_display.setForeground(new Color(53,48,201));//글자색
		jbtn_new.setBackground(new Color(181,178,255));
		jbtn_new.setForeground(new Color(0,0,99));
		jbtn_clear.setBackground(new Color(181,178,255));
		jbtn_clear.setForeground(new Color(0,0,0));
		jbtn_dap.setBackground(new Color(181,178,255));
		jbtn_dap.setForeground(new Color(0,0,99));
		jbtn_exit.setBackground(new Color(181,178,255));
		jbtn_exit.setForeground(new Color(0,0,99));
		jtf_user.setBackground(new Color(243,97,166));
		
		jf.setLayout(new BorderLayout(10,20));
		jp_center.setLayout(new BorderLayout(0,10));
		
		jp_center.setLayout(new BorderLayout());//center panel을 배경 레이아웃으로 만들기
		//jp_center.add(jta_display);
		jp_east.setLayout(new GridLayout(4,1));//행을 4로 똑같이 나누는 레이아웃
		
		jta_display.setLineWrap(true);//가로 텍스트가 넘칠때 자동 줄바꿈
		
		jp_east.add(jbtn_new);
		jp_east.add(jbtn_clear);
		jp_east.add(jbtn_dap);
		jp_east.add(jbtn_exit);
		jp_center.add("Center",jsp_display);//TextArea를 가진 스크롤 속지 올리기
		jp_center.add("South",jtf_user);
		jf.add("Center",jp_center);
		jf.add("East",jp_east);//jp_east를 JFRAME의 동쪽에 넣어주세요.
		
		jf.setTitle("야구 숫자 게임 Ver1.0");
		jf.setSize(400, 300);
		jf.setVisible(true);
	}
	
	public static void main(String[] args) {
		BaseBallGame bbGame = new BaseBallGame();
		bbGame.initDisplay();
	

	}

	@Override
	public void actionPerformed(ActionEvent e) {
		if(e.getSource()==jtf_user) {
			jta_display.append(jtf_user.getText()+"\n");
			jtf_user.setText("");
		}//end of if
		
	}

}

```

### 숙제

#### 피보나치 수열의 규칙을 찾아서 a1, a2, a3, ..., a10항까지 출력해주는 프로그램을 작성하시.\(for, if 사\)

```java
package test;

public class Test08052 {
	//피보나치 수열의 규칙을 찾아서 a1, a2, a3, ..., a10항까지 출력해주는 프로그램을 작성하시.(for, if 사)

	public static void main(String[] args) {
		
		int i;
		int num1, num2, sum;
		num1 = 0;
		num2 = 1;
		sum = 1;
		
		for(i=0;i<=10;i++) {			
			
			System.out.println(sum);//1,1,2,3,5
			sum = num1 + num2;//1,2,3,5
			num1 = num2;//num1=1,1,2,3
			num2 = sum;//num2=1,2,3,5
			
			
		}//end of for

	}

}

```

후기 : 중간중간 예제문제를 내주시고 배운것들과 검색을 활용하여 순서도를 그려보고, 코드를 혼자 짜서 결과가 나오면 뿌듯하다. 많은 예제에 도전해보자.

