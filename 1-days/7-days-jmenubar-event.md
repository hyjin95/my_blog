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
	package game.baseball;

import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JPanel;
import javax.swing.JScrollPane;
import javax.swing.JTextArea;
import javax.swing.JTextField;
import javax.swing.JMenu;
import javax.swing.JMenuBar;
import javax.swing.JMenuItem;

import java.awt.BorderLayout;
import java.awt.Color;
import java.awt.Font;
import java.awt.GridLayout;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.beans.EventHandler;
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
	JTextArea   jta_display = new JTextArea();
	JTextField  jtf_user    = new JTextField();
	JScrollPane jsp_display = new JScrollPane(jta_display //스크롤속지위에 TextArea올리기
                    			, JScrollPane.VERTICAL_SCROLLBAR_AS_NEEDED//세로스크롤바는 필요한 상황에 생성하기
			                    , JScrollPane.HORIZONTAL_SCROLLBAR_NEVER); //가로스크롤 생성하지 않기
	
	//글꼴과 글꼴에 대한 스타일과 글자 크기를 인스턴스화를 통해서 정해준다.
	//그 값들은 생성자의 파라미터로 결정된다.(글꼴,스타일,크기)
	Font f = new Font("Thoma",Font.BOLD,14);
	
	//동쪽에 들어갈 속지 생성
	JPanel jp_east = new JPanel();//버튼을 넣으면 width가 늘어난다.
	//east속지에 버튼 추가하기.(새게임, 정답, 지우기, 나가기)
	JButton jbtn_new  = new JButton("새게임");
	JButton jbtn_dap  = new JButton("정답");
	JButton jbtn_clear= new JButton("지우기");
	JButton jbtn_exit = new JButton("나가기");
	
	JMenuBar  jmb  = new JMenuBar();	
	JMenu jm_file  = new JMenu("File");
	JMenu  jm_info = new JMenu("Information");

	JMenuItem jmi_new    = new JMenuItem("새 게임");
	JMenuItem jmi_dap    = new JMenuItem("정답");
	JMenuItem jmi_clear  = new JMenuItem("지우기");
	JMenuItem jmi_exit   = new JMenuItem("나가기");
	JMenuItem jmi_detail = new JMenuItem("야구숫자게임 소개");
	JMenuItem jmi_create = new JMenuItem("만든사람들");
	
	//나가기 버튼이나 나가기 메뉴아이템을 클릭했을때 호출되는 메소드 구현
	public void exit() {
		System.out.println("exit 호출 성공");
		System.exit(0);
		}//end of exit메소드
	
	//화면을 그려주는 메소드 선언
	public void initDisplay( ) {
		System.out.println("initDisplay 호출 성공");
		//이벤트 소스와 이벤트 처리 클래스를 매칭, 연결해주는 코드 추가
		//EventHandler ehandler = new EventHandler();
		//jtf_user.addActionListener(ehandler);
		jtf_user.addActionListener(this);//여기서 this는 자기 자신 클래스를 가르킨다.=BaseBallGame:내안 어디엔가에 actionPerformed가 있어야 한다.
		jbtn_new.addActionListener(this);
		jbtn_dap.addActionListener(this);
		jbtn_clear.addActionListener(this);
		jbtn_exit.addActionListener(this);
		jmi_exit.addActionListener(this);
		
		
		//////////메뉴바 추가 시작///////////
		jm_file.add(jmi_new);		
		jm_file.add(jmi_dap);		
		jm_file.add(jmi_clear);		
		jm_file.add(jmi_exit);		
		jm_info.add(jmi_detail);		
		jm_info.add(jmi_create);				
		jm_file.setMnemonic('f');
		jm_info.setMnemonic('i');
		jmb.add(jm_file);
		jmb.add(jm_info);
		//////////메뉴바 추가 끝///////////
				
		JFrame jf = new JFrame();
		
		jta_display.setFont(f);
		
		jp_center.setBackground(Color.lightGray);
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
		
		
		jp_center.setLayout(new BorderLayout());//center panel을 배경 레이아웃으로 만들기
		//jp_center.add(jta_display);
		jp_east.setLayout(new GridLayout(4,1));//행을 4로 똑같이 나누는 레이아웃
		
		jta_display.setLineWrap(true);//가로 텍스트가 넘칠때 자동 줄바꿈
		
		jp_center.setLayout(new BorderLayout(0,10));
		jf.setLayout(new BorderLayout(10,20));
		
		jp_east.add(jbtn_new);
		jp_east.add(jbtn_clear);
		jp_east.add(jbtn_dap);
		jp_east.add(jbtn_exit);
		jp_center.add("Center",jsp_display);//TextArea를 가진 스크롤 속지 올리기
		jp_center.add("South",jtf_user);
		jf.add("Center",jp_center);
		jf.add("East",jp_east);//jp_east를 JFRAME의 동쪽에 넣어주세요.
		
		jf.setJMenuBar(jmb);
		jf.setTitle("야구 숫자 게임 Ver1.0");
		jf.setSize(400, 300);
		jf.setVisible(true);
	}
	
	public static void main(String[] args) {
		BaseBallGame bbGame = new BaseBallGame();
		bbGame.initDisplay();	

	}//end of main
	/////jtf_user에 엔터를 쳣을때, jbtn_exit버튼을 클릭했을때 이번트를 지원하는 인터페이스가 ActionListener이다.
	//ActionListener는 반드시 actionPerformed를 재정의 해야 한다.
	//콜백메소드는 개발자가 호출할 수 없는 메소드로 시스템 레벨에서 필요할때 자동으로 호출된다.
	//자바에  main메소드도 일종의 콜백 메소드이다.
	@Override//@=annotation - 부모가 가진 메소드를 재정의 했다는 의미이다.
	public void actionPerformed(ActionEvent e) {
		//System.out.println("actionPerformed 호출 성공");
		String lavel = e.getActionCommand();		
		System.out.println("너가 누른 버튼의 문자열은 "+lavel+" 입니다.");
		Object obj = e.getSource();//이벤트 소스의 주소번지를 담아준다.
		//너 나가기 버튼이니?
		if("나가기".contentEquals(lavel)) {//if없으면 입력후 enter사용시 나가진다.
			exit();
		}
		if(obj==jbtn_exit || obj==jmi_exit) {//주소번지를 사용하는 방법, else if면 위의조건에서 내려오지 않는다.
			exit();
		}
		
		else if(e.getSource()==jtf_user) {//jtf_user에 이벤트 발생시 enter을 사용하면 화면에 출력
			jta_display.append(jtf_user.getText()+"\n");
			jtf_user.setText("");
		}//end of if
		
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

