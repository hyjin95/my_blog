# Main1.java

```java
package book.ch5;

import java.awt.Container;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JOptionPane;
import javax.swing.JTextField;

public class Main1 extends JFrame implements ActionListener {
	//선언부 : 전변
	JButton    jbtn_south  = null;//class안에 선언
	JButton    jbtn_north  = null;
	JButton    jbtn_east   = null;
	JButton    jbtn_west   = null;
	JButton    jbtn_center = null;
	JTextField jtf_user    = null;//버튼대신 들어갈 jtf선언, 다른 클래스나 메서드에서도 재사용할수 있도록 class안에 전역번수로 선언한다.생성은 사용되는 이벤트performed안에서 한다.
	
	Container cont = this.getContentPane();

	//생성자 
	//리턴타입이 없다. 파라미터는 n개 가능하다. 메서드 오버로딩의 규칙을 준수한다. 전역변수의 초기화를 담당한다.
	public Main1( ) {//디폴트생성자, 초기화하는 역할
		jbtn_east   = new JButton("동쪽(버튼)");//생성자 안에 생성
		jbtn_south  = new JButton("남쪽(버튼)");
		jbtn_center = new JButton("중앙(버튼)");
		jbtn_west   = new JButton("서쪽(버튼)");
		jtf_user    = new JTextField();//사용할 곳에서 생성한다.
		//내안의 메서드, 전연변수는 인스턴스화없이 호출가능하다.
		initDisplay();//initDisplay가 버튼 위에 있으면 버튼이 null값을 가질 수도 있다.
	}
	
	//화면처리부
	public void initDisplay() {
		jbtn_south.addActionListener(this);
		jbtn_east.addActionListener(this);
		jbtn_west.addActionListener(this);
		jtf_user.addActionListener(this);
		this.add("East", jbtn_east);
		this.add("West", jbtn_west);
		this.add("Center", jbtn_center);
		this.add("South", jbtn_south);
		this.setSize(500, 400);
		this.setVisible(true);
	}

	//메인 메서드	
	public static void main(String[] args) {
		//여기에 인스턴스화를 하면 지역변수의 성격을 갖게되므로 외부에서 사용 불가하다.
		new Main1();//생성자 호출하기
	}

	//콜백메서드
	//시스템에서 자동으로 호출 됨 (감지하면 JVM이 호출)
	@Override
	public void actionPerformed(ActionEvent e) {
		Object obj = e.getSource();//이벤트감지
		String msg = jtf_user.getText();//사용자가 입력한 문자열 읽어오기, jtf_user가 사용되기 전에 생성되어야한다.
		if(obj==jtf_user) {//사용자가 입력한 문자열을 이벤트 감지했을때
			if("북쪽".equals(msg)) {
				jbtn_north = new JButton("북쪽(버튼)");//이벤트가 일어난 후에 생성하므로 여기에 생성
				cont.add("North",jbtn_north);
				cont.revalidate();
			}//end of if											
			else {
				JOptionPane.showMessageDialog(this, "북쪽을 입력해 주세요.");
				jtf_user.setText("");
				return;
			}
		}//end of if
		else if(obj==jbtn_west) {
			if(jbtn_center!=null) {
			cont.remove(jbtn_center);
			}
			if(jbtn_west!=null) {
			cont.remove(jbtn_west);
			}
			if(jbtn_north!=null) {
				cont.remove(jbtn_north);
			}
			if(jbtn_east!=null) {
			cont.remove(jbtn_east);
			}
			if(jtf_user!=null) {
				cont.remove(jtf_user);
			}
			cont.revalidate();			
		}
		else if(obj==jbtn_east) {
			jbtn_east.setText("메뉴");
		}
		
		else if(obj==jbtn_south) {
			System.out.println("남쪽버튼 클릭");
			
			//java에서는 remove했다고 삭제되는것이 아니라 Candidate상태로
			//전환한다.=쓰레기값, 곧 내가 청소할 거라고 찜해놓는것.
			cont.remove(jbtn_south);//버튼을 전환할거야
			System.out.println(jbtn_south);//주소번지 출력됨
			jbtn_south = null;//초기화 : JVM이 기억하고 있는 주소번지를 지워줌 =Candidate상태로 할거다라는 의미
			//끼어들 위치를 설정해야한다.
			
			//initDisplay에서 이벤트 핸들러와 연결하지 않고
			//이벤트가 발동해야 인스턴스화를 완성한다. 
			//jtf_user = new JTextField();//남쪽버튼을 누르면 생성된다.
			//jtf.actionLitener(this);
			cont.add("South",jtf_user);//새로 붙이기
			cont.revalidate(); 				
		}//end of if	
		
	}//end of actionPerformed
}//end of class

//현재 선언과 생성을 따로 분리하는것은 따로 차이가 없다. 
//향후에 화면구현, 또는 이벤트 처리시 따로 분리하는 부분이 필요해서 연습한다.
//다만 생성자 안에서 initDisplay를 호출함에 따라 initDisplay를 
//먼저 호출하면 해당 버튼을 보지 못할 수도 있다.
```

*
  * 25번 : 생성자 생성
  * 50번 : 메인 메서드에서 생성자 호출
  * 32번 : 생성자에서 화면처리 메서드 호출
  * 36번 : 화면처리 메서드에서 화면설정
  * 14-19번 : 버튼및 입력TextField 선언\
    \- 선언은 class 범위에서, 생성은 생성자 안에서, 구현은 화면처리안에서
  * 41-44번 : 버튼 화면에 호출
  * 26-30번 : 생성자 안에서 버튼및 입력Field 생성
  * 37-40번 : 사용할 이벤트에 해당되는 버튼 및 입력Field 감지 코드 작성\
    \- = enentHandler와 매칭\
    \- 이벤트 생성시 : actionListener, 클래스에 implement actionListener, 오버라이드&#x20;
  * 58번 : 이벤트 처리 콜백메서드
  * 59번 : obj = 감지된 이벤트
  * 60번 : msg = TextField에서 입력된 문자
  * 61-66번 : TextField에 "북쪽"입력시 버튼이 나타나는 이벤트
  * 67-70번 : TextField에 "북쪽"외 다른 글 입력시 알림창과 Field를 리셋해주는 이벤트
  * 73-90번 : "서쪽(버튼)"클릭시 모든 버튼을 없애는 이벤트&#x20;
  * 91-93번 : "동쪽(버튼)"클릭시 "메뉴"로 문자열을 변경하는 이벤트
  * 95-110번 : "남쪽(버튼)"클릭시 TextField 입력창으로 바꿔주는 이벤트\
    \- 사용 클래스 : Container cont = this.getContentPane(); \
    &#x20; (API : JFrame이 붙어있는 캔버스)\
    \- **remove(삭제할 화면정보);** : **** 클래스안의 메서드 \
    \- 삭제할 화면정보=null;까지 해주어야 주소번지까지 Candidate상태로 변환한다.\
    \- **Candidate** : 쓰레기값, 곧 전환될 값\
    \- **revalidate()**; : 갱신메서드

