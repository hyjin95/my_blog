# 야구 숫자 게임 코드

```java
package game.baseball;

import javax.swing.ImageIcon;
import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JPanel;
import javax.swing.JScrollPane;
import javax.swing.JTextArea;
import javax.swing.JTextField;
import javax.swing.JToolBar;
import javax.swing.JMenu;
import javax.swing.JMenuBar;
import javax.swing.JMenuItem;
import javax.swing.JOptionPane;

import java.awt.BorderLayout;
import java.awt.Color;
import java.awt.Font;
import java.awt.Graphics;
import java.awt.GridLayout;
import java.awt.Image;
import java.awt.Point;
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
	JFrame jf = new JFrame();
	//이미지를 담은 물리적인 위치 선언하기
	String imgPath = "C:\\workspace_java\\dev_java\\src\\game\\baseball\\";
	//ImageIcon titleIcon = new ImageIcon(imgPath+"");
	//ImageIcon bg = new ImageIcon(imgPath+"big.png");
	Image img = jf.getToolkit().getImage(imgPath+"big.png");
	//이미지 버튼을 만드는데 필요한 클래스 선언하기	
	//중앙에 들어갈 속지 선언, 전변
	JPanel jp_center = new JPanel();
	
	JScrollPane jsp_display = null;
	//세자리 숫자를 입력후 enter를 쳤을때 사용자가 입력한 숫자와 숫자를 맞추기 위한 힌트문을 출력해줄 화면
	JTextArea   jta_display = null;
	JTextField  jtf_user    = new JTextField();
	
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
	JButton jbtn_inew = new JButton();
	
	JMenuBar  jmb  = new JMenuBar();	
	JMenu jm_file  = new JMenu("File");
	JMenu  jm_info = new JMenu("Information");

	JMenuItem jmi_new    = new JMenuItem("새 게임");
	JMenuItem jmi_dap    = new JMenuItem("정답");
	JMenuItem jmi_clear  = new JMenuItem("지우기");
	JMenuItem jmi_exit   = new JMenuItem("나가기");
	JMenuItem jmi_detail = new JMenuItem("야구숫자게임 소개");
	JMenuItem jmi_create = new JMenuItem("만든사람들");
	
	JToolBar  toolbar    = new JToolBar();
	
	int my[] = new int[3];
	int com[] = new int[3];
	int cnt = 0;//++cnt : 힌트 문장에서 순번을 출력하는 변수
	/* 배경이미지구현
	class BgPanel extends JPanel{
		public void paintComponent(Graphics g) {
			g.drawImage(bg.getImage(), 0,0,null);
			setOpaque(false);
			super.paintComponent(g);
		}
	}*/
	
	//세자리 임의의 숫자를 채번하는 메소드 구현하기
	public void ranCom() {
		com[0] = (int)(Math.random()*10);;//0-9사이의 임의의 정수
		do {
			com[1] = (int)(Math.random()*10);;	
		}while(com[0]==com[1]);
		do {
			com[2] = (int)(Math.random()*10);;				
		}while(com[0]==com[2] || com[1]==com[2]);		
	}//end of ranCom
	
	
	//사용자가 입력한 값을 판정하는 메소드 구현
	public String account(String user) {
		
		if(user.length()!=3) {
			return "세자리 숫자를 입력하세요.";
		}
		//사용자가 jtf_user에 입력한 숫자는 숫자처럼 보이지만 알맹이는 문자열로 인식한다.
		//그래서 형전환 후 my[]배열에 담아야 한다.
		//문자열 "256"을 숫자로 담을 변수 선언
		int temp   = 0;
		int strike = 0;//힌트로 사용될 스트라이크를 담을 변수 선언
		int ball   = 0;//힌트로 사용될 볼을 담을 변수 선언
		//strike와 ball을 지역변수로 해야하는 것(초기화를 해야하는 이유)은 매 회차 마다 값이 변해야 하기 때문이다.
		
		try {
			temp = Integer.parseInt(user);
		} catch (NumberFormatException e) {
			return "숫자만 입력하세요.";
		}
		//temp = Integer.parseInt(user);
		my[0] = temp/100;
		my[1] = (temp%100)/10;
		my[2] = temp%10;
		JOptionPane.showMessageDialog(null, my[0]+""+my[1]+""+my[2]);
		
		for(int i=0;i<3;i++) {
			for(int j=0;j<3;j++) {
				if(my[i]==com[j]) {//너 안에 내가 가진 숫자가 있니?
					if(i==j) {//그러면 자리까지도 일치하니?
						strike++;
					}//end of if
					else ball++;					
				}//end of if
			}//end of for
		}//end of for
		
		if(strike==3) {
			return "축하합니다. 정답입니다!";			
		}
		return strike+"스"+ball+"볼";
	}
	
	//나가기 버튼이나 나가기 메뉴아이템을 클릭했을때 호출되는 메소드 구현
	public void exit() {
		System.out.println("exit 호출 성공");
		System.exit(0);
		}//end of exit메소드
	
	//화면을 그려주는 메소드 선언
	public void initDisplay( ) {
		jta_display = new JTextArea() {
			public void paint(Graphics g) {
				g.drawImage(img, 0, 0, null);
				Point p = jsp_display.getViewport().getViewPosition();
				g.drawImage(img, p.x, p.y,null);//p.x : 스크롤바의 위치에 대한 좌표
				paintComponent(g);
			}
		};
		jsp_display = new JScrollPane(jta_display);
		//jf.setContentPane(new BgPanel());
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
		jmi_new.addActionListener(this);
		///////////////툴바에 들어갈 이미지 버튼 추가하기//////////////
		//jbtn_inew.setIcon(new ImageIcon(imgPath+"dusan_bears.jpg"));
		toolbar.add(jbtn_inew);		
		
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
		jf.setLayout(new BorderLayout(10,30));
		
		jf.add("North",toolbar);
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
		BaseBallGame bbg = new BaseBallGame();
		bbg.initDisplay();
		bbg.ranCom();//컴퓨터가 채번한 숫자가 채워진다. 호출 전에는 0,0,0

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
		/*if(obj==jbtn_exit || obj==jmi_exit) {//주소번지를 사용하는 방법, else if면 위의조건에서 내려오지 않는다.
			exit();
		}*/
		//이벤트가 발생한 이벤트 소스의 문자열을 비교하기
		else if(e.getSource()==jtf_user) {//jtf_user에 이벤트 발생시 enter을 사용하면 화면에 출력
			jta_display.append(++cnt+"회차. "+jtf_user.getText()+" : "+account(jtf_user.getText())+"\n");
			jtf_user.setText("");
		}//end of if
		else if(obj==jbtn_dap) {
			jta_display.append("정답은"+com[0]+com[1]+com[2]+"\n");
		}///입력하고 엔터 쳤을때
		//새게임 : 채번, cnt 초기화
		else if(obj==jbtn_new || obj==jmi_new) {
			cnt = 0;
			ranCom();
			jta_display.setText("");
			jtf_user.setText("");
			jtf_user.requestFocus();
			
			
		}
		
	}////end of actionPerformed

}
```

