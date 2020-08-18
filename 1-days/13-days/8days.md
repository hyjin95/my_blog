# 8Days 야구숫자게임 쪼개기

## BaseBallGame

### Main

```java
package baseballgame;

import baseballgame.BaseBallGameView;

public class BaseBallGameMain {

	public static void main(String[] args) {
		BaseBallGameView bbgv = new BaseBallGameView();
		bbgv.initDisplay();
		bbgv.bgl.ranCom();	
	}
}
```

### View

```java
package baseballgame;

import java.awt.BorderLayout;
import java.awt.Color;
import java.awt.Font;
import java.awt.Graphics;
import java.awt.GridLayout;
import java.awt.Image;
import java.awt.Point;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

import javax.swing.ImageIcon;
import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JMenu;
import javax.swing.JMenuBar;
import javax.swing.JMenuItem;
import javax.swing.JPanel;
import javax.swing.JScrollPane;
import javax.swing.JTextArea;
import javax.swing.JTextField;
import javax.swing.JToolBar;


public class BaseBallGameView extends JFrame{
	BaseBallGameEvent bge = new BaseBallGameEvent(this);
	BaseBallGameLogic bgl = new BaseBallGameLogic();
	
	JFrame    jf         = new JFrame();
	
	String    imgPath = "";
	Image img = null;
	
	JButton   jbtn_inew  = new JButton();
	JMenuBar  jmb        = new JMenuBar();
	
	JMenu     jm_game    = new JMenu("게임");
	JMenuItem jmi_new    = new JMenuItem("새게임");
	JMenuItem jmi_dap    = new JMenuItem("정답");
	JMenuItem jmi_clear  = new JMenuItem("지우기");
	JMenuItem jmi_exit   = new JMenuItem("나가기");
	
	JMenu     jm_info    = new JMenu("도움말");
	JMenuItem jmi_detail = new JMenuItem("야구숫자게임이란?");
	JMenuItem jmi_create = new JMenuItem("만든사람들");
	
	JToolBar toolbar     = new JToolBar();
	JPanel jp_center   = new JPanel(); //중앙 속지
	JTextArea jta_display = null; // 중앙 전광판 화면
	JScrollPane jsp_display = new JScrollPane(JScrollPane.VERTICAL_SCROLLBAR_AS_NEEDED, 
											  JScrollPane.HORIZONTAL_SCROLLBAR_NEVER); //스크롤
	JTextField jtf_user    = new JTextField(); //입력창
	Font f = new Font("Thoma",Font.BOLD,14); //글꼴
	
	JPanel jp_east     = new JPanel();
	
	JButton jbtn_new     = new JButton("새게임");
	JButton jbtn_dap     = new JButton("정답");
	JButton jbtn_clear   = new JButton("지우기");
	JButton jbtn_exit    = new JButton("나가기"); 
	
	public BaseBallGameView() {
		
	}//end of BaseBallGameView 생성자
	public void initDisplay() {
		
		img = jf.getToolkit().getImage(imgPath+"dreamballpark.jpg");
		jta_display   = new JTextArea() {
		public void paint(Graphics g) {
		g.drawImage(img, 0, 0, null);
		Point p = jsp_display.getViewport().getViewPosition();
		g.drawImage(img, p.x, p.y, null);
		paintComponent(g);
		}
		};
		
		jsp_display = new JScrollPane(jta_display);
		jta_display.setOpaque(false);//setOpaque: 투명하게
		
		jbtn_inew.setIcon(new ImageIcon(imgPath+"DOOSAN_BEARS.png"));//툴바에 들어갈 이미지
		toolbar.add(jbtn_inew);
		
		jm_game.add(jmi_new);//메뉴바 
		jm_game.add(jmi_dap);
		jm_game.add(jmi_clear);
		jm_game.add(jmi_exit);
		jm_info.add(jmi_detail);
		jm_info.add(jmi_create);
		jmb.add(jm_game);
		jmb.add(jm_info);		
		
		System.out.println("initDisplay 호출 성공");
		
		jtf_user.addActionListener(bge);//여기서 this는 자기자신 클래스를 가리킴.-BaseBallGame:내안에 actionPerformed
		jbtn_new.addActionListener(bge);//addActionListener있어야 감지가 있다.
		jbtn_dap.addActionListener(bge);
		jbtn_clear.addActionListener(bge);
		jbtn_exit.addActionListener(bge);
		jmi_exit.addActionListener(bge);
		jmi_new.addActionListener(bge);
		
		jbtn_new.setBackground(new Color(255,217,250));
		jbtn_new.setForeground(new Color(3,0,102));
		jbtn_dap.setBackground(new Color(255,217,250));
		jbtn_dap.setForeground(new Color(3,0,102));
		jbtn_clear.setBackground(new Color(255,217,250));
		jbtn_clear.setForeground(new Color(3,0,102));
		jbtn_exit.setBackground(new Color(255,217,250));
		jbtn_exit.setForeground(new Color(3,0,102));
		
		jp_east.setLayout(new GridLayout(4,1)); //높이 같게(행,열) 
		jp_east.add(jbtn_new);
		jp_east.add(jbtn_dap);
		jp_east.add(jbtn_clear);
		jp_east.add(jbtn_exit);
		
		jta_display.setFont(f);
		jta_display.setBackground(new Color(217,229,255));//배경색 바꿔주는 것 
		jta_display.setForeground(new Color(5,0,153));//글자색 바꿔주는 것
		
		jf.setJMenuBar(jmb);
		jtf_user.setBackground(new Color(217,229,255));
		jp_center.setBackground(Color.black);
		jp_east.setBackground(Color.gray);//Color.색상 : 종이에 색칠하는 용어
		//center panel을 배경 레이아웃으로 만들기
		jp_center.setLayout(new BorderLayout(0,10));
		
		jp_center.add("Center",jsp_display); //스크롤바 생성해주는 것 : jsp
		jp_center.add("South",jtf_user);
		
		jta_display.setLineWrap(true);//텍스트 넘치면 자동으로 줄이 바꿔지게 하는 것 
		jf.setLayout(new BorderLayout(10,20)); //버튼과 중간화면 세로 간격
		jf.add("North",toolbar);
		jf.add("Center",jp_center);
		jf.add("East",jp_east);
		
		jf.setJMenuBar(jmb);
		jf.setTitle("야구 숫자 게임 Ver1.0");//화면이름
		jf.setSize(400,300);//화면크기
		jf.setVisible(true);//창에 화면을 나타낼 것 인지 확인하는 것
	}//end of initDisplay
	
}//end of BaseBallGameView	 
```

### Event

```java
package baseballgame;

import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;


public class BaseBallGameEvent implements ActionListener {
	BaseBallGameView bmv = null;
	

	public BaseBallGameEvent(BaseBallGameView bmg) {
		this.bmv = bmg;
	}	
	
	@Override
	public void actionPerformed(ActionEvent e) {
		BaseBallGameCRUD bcrud = new BaseBallGameCRUD();
		String label = e.getActionCommand();//입력 or수정 or상세보기가 들어옴
		//System.out.println("lable = "+label);//단위테스트
		Object obj = e.getSource();
		
		if("나가기".equals(label)) {
			System.exit(0);
		}
		
		else if("새게임".equals(label)) {//글자타입 비교는 .equals
			bmv.bgl.cnt = 0;
			bmv.bgl.ranCom();
			bmv.jta_display.setText("");
			bmv.jtf_user.setText("");
			bmv.jtf_user.requestFocus();
		}
		
		else if("정답".equals(label)) {
			bmv.jta_display.append("정답은"+bmv.bgl.com[0]+bmv.bgl.com[1]+bmv.bgl.com[2]+"\n");
		}		
		else if("지우기".equals(label)) {
			
		}		
		else if(obj==bmv.jmi_detail) {//야구숫자게임이란 대응이벤트
			bcrud.initDisplay();
			bcrud.setTitle("야구숫자게임이란?");
			bcrud.bjta_display.append("야구숫자게임은 랜덤한 세 숫자를 맞추는 게임입니다. "
					                   +"\n" +"숫자와 자리를 맞추면 Strike, 하나만 맞추면 Ball이 카운트됩니다.");
			bcrud.setSize(200, 300);
			bcrud.setVisible(true);
			
		}
		else if(obj==bmv.jmi_create) {//만든사람들 대응이벤트
			bcrud.initDisplay();
			bcrud.bjta_display.append("KOSMO69기"+"\n"+"오상기"+"\n"+"윤주연"+"\n"+"신민경"+"\n"+"한영진");
			bcrud.setTitle("제작자");
			bcrud.setSize(200, 300);
			bcrud.setVisible(true);
			
		}
		else if(e.getSource()==bmv.jtf_user) {
			bmv.jta_display.append(++bmv.bgl.cnt+"회차. "+bmv.jtf_user.getText()+" : "+bmv.bgl.account(bmv.jtf_user.getText())+"\n");
			bmv.jtf_user.setText("");
		}
			
	  }//end of actionPerformed
}
```

### Logic

```java
package baseballgame;

 import javax.swing.JOptionPane;

public class BaseBallGameLogic {
	
    int my[]  = new int[3];
    int com[] = new int[3];
    int cnt = 0;
    
    public void ranCom() {
        com[0] =(int)(Math.random()*10);
        do {
           com[1] =(int)(Math.random()*10);         
        }while(com[0]==com[1]);
        do {
           com[2] =(int)(Math.random()*10);         
        }while(com[0]==com[2] || com[1]==com[2]);
     }
     public String account(String user) {
       if(user.length()!=3) {
          return "세자리 숫자를 입력하세요.";
       }
       //사용자가 jtf_user에 입력한 숫자는 보기에는 숫자 처럼 보여도 알맹이는 문자열로 
      //인식을 합니다. 그래서 형전환을 한 후 my[]배열에 담아야 함니다.
       //문자열 "256"을 숫자로 담을 변수 선언
      int temp    = 0;
       int strike = 0;//힌트로 사용될 스트라이크를 담을 변수 선언
      int ball    = 0;//볼을 담을 변수 선언
      //strike와ball을 지역변수로 해야 하는건 매 회차 마다 값이 변해야 하기 때문이다.
       try {
          temp = Integer.parseInt(user);
       } catch (NumberFormatException e) {
          return "숫자만 입력하세요.";
       }
       
       my[0] = temp/100;
       my[1] = (temp%100)/10;
       my[2] = temp%10;
       JOptionPane.showMessageDialog(null, my[0]+""+my[1]+""+my[2]);
       for(int i=0;i<3;i++) {
          for(int j=0;j<3;j++) {
             if(my[i]==com[j]) {//너 안에 내가 가진 숫자가 있는거야?
                if(i==j)//그러면 자리까지도 일치하는 거니?
                   strike++;
                else ball++;
             }
          }
       }
       if(strike==3) {
          return "축하합니다. 정답입니다!!!";
       }
       return strike+"스  "+ball+"볼";
    }  	
}    
```

### CRUD

```java
package baseballgame;

import java.awt.Color;

import javax.swing.JDialog;
import javax.swing.JFrame;
import javax.swing.JTextArea;

public class BaseBallGameCRUD extends JDialog {
	JTextArea bjta_display = null;
	
	public void initDisplay() {
		bjta_display = new JTextArea();
		add(bjta_display);
		bjta_display.setLineWrap(true);
		bjta_display.setBackground(new Color(255,220,239));//배경색
		bjta_display.setForeground(new Color(53,48,201));//글자색
		this.setVisible(false);
	}
	/*
	 * public static void main(String args[]) { new BookCRUD().initDisplay(); }
	 */
}

```

