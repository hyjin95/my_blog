# Exam1.java

### 선언부분

```java
package desigin.test;
import java.awt.BorderLayout;
import java.awt.Container;
import java.awt.FlowLayout;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

import javax.swing.JButton;
/*인스턴스화 할 수 있다. (A a = new A(), A a = methodA();)
 * 인스턴스 변수를 활용하여 변수사용(전변) 및 메서드를 호출(파라미터, 리턴타입)할 수 있다. 
 * UI부분과 업무처리 부분을 나눈다.
 * 화면과 로직을 분리해야 한다.
 * 신입 업무의 비중
 * 화면과 이벤트 처리 기준 : 전체의 50% (소통-업무시작-리턴타입과 파라미터)
 * 업무내용 : 전체의 20% (업무에 대한 복잡도-조인이 거의 없거다 단순 조인문장)
 * DB연동 및 업무처리 : 30% 
 */
import javax.swing.JFrame;
import javax.swing.JOptionPane;
import javax.swing.JPanel;
import javax.swing.JScrollPane;
import javax.swing.JTable;
import javax.swing.JTextField;
import javax.swing.table.DefaultTableModel;
//구현체 클래스=eventHandler클래스
public class Exam1 extends JFrame implements ActionListener {
	//선언부 - 주의 : 선언부에서 메서드 호출이나 값에 대한 재정의는 불가능
	//테이블 추가하기
	JTable            jtb_list = null;//버튼을 누르면 table생성 = 선언만하고 이벤트에서 생성=양식안의 테이블
	JScrollPane       jsp_list = null;//jtable에 스크롤바 붙이기
	DefaultTableModel dtm_list = null;//jtable안의 데이터 설정에 필요한 클래스 = 기본 비닐 양식
	//테이블은 row와 colum이 만나야 면이되고 면이 되어야 값을 넣을 수 있다. = 이중배열이 필요
	String            cols[]   = {"성명","자바","오라클","HTML","총점","평균","석차"};//colum명
	String            data[][] = null;//앞[]이row, 학생수가 결정되지 않아서 생성
	
	JPanel jp_north = new JPanel();//textfield와 버튼을 담는 속지
	JPanel jp_south = new JPanel();
	JTextField jtf_inwon = new JTextField();//화면이 열리자마자 보여져야 하니까 생성까지한다.
	JButton jbtn_create  = new JButton("생성");
	JButton jbtn_account = new JButton("처리");
	JButton jbtn_exit    = new JButton("종료");
	int inwon = 0;//getter로 jtf_inwon의 값을 가져와야한다.
	
	//화면을 갱신할때에는 Container를 활용하자 =제일 밑바닥의 속지
	Container cont = this.getContentPane();
	
```

* import, 클래스, 멤버변수 선언
* 26번 : ActionListener인터페이스를 implements하고 JFrame추상클래스를 상속받는 클래스 정의
* 29-31, 36-41번 : UI 선언
* 33-34, 42번 : 데이터를 담는 배열 선언
* 45번 : 이벤트처리시 화면 갱신을 위한 Container클래스 선

### 생성자와 화면구현, 필요한 메서드 작성해보기

```java
	//생성자
	public Exam1() { }	
	//화면처리부
	public void initDisplay() {
		jbtn_create.addActionListener(this);//없으면 actionPerformed는 호출되지 않는다.
		jbtn_account.addActionListener(this);
		jbtn_exit.addActionListener(this);//이벤트가 감지되면 JVM이 eventPerformed를 호출
		
		//속지의 layout을 borderLayout으로 해야한다. center에 jtf_inwon을 east jbtn_create를 담기
		jp_north.setLayout(new BorderLayout());
		jp_north.add("Center", jtf_inwon);
		jp_north.add("East", jbtn_create);
		//남쪽 속지 레이아웃 flowLayout으로 설정한다. - 파라미터로 정렬이 가능
		jp_south.setLayout(new FlowLayout(FlowLayout.RIGHT));//오른쪽으로 정렬
		jp_south.add(jbtn_account);
		jp_south.add(jbtn_exit);
		this.add(jbtn_exit);
		this.add("North", jp_north);
		this.add("South", jp_south);
		this.setSize(500, 400);
		this.setVisible(true);		
	}
	
	//성적처리 메서드 구현 : 업무처리, 알고리즘
	public void account() {
		System.out.println("성적처리메서드 호출성공");
	}

	//메인메서드=main thread, 라이프 사이클 관리 : start() service(run)() destroy()
	public static void main(String[] args) {
		//생각해볼 문제 : initDisplay()를 메인메서드에서 호출하는것과 생성자에서 호출하는것의
		//차이에는 어떤 차이가 있는가
		Exam1 e1 = new Exam1();//생성자에서 화면 메서드 호출
		e1.initDisplay();//메인메서드에서 화면 메서드 호출
	}	
```

* 2번 : 디폴트생성
* 4번 : 화면처리 메서드
* 5-7번 : 이벤트 발생 감지하는 메서드 선언. 소유주의 이벤트가 감지되면 JVM이 eventPerformed를 호출
* 10-19번 : 필요한 속지와 버튼, TextField등 붙이기
* 20-21번 : 화면 구성 
* 25번: 추후에 성적처리할 메서드 정의
* 30번 : 메인 메서드
* 33번 : 2번의 디폴트 생성자 호출\( initDisplay\(\);이 있으면 33번에서 호출된다.\)
* 34번 : 인스턴스화, 인스턴스 변수를 이용한 화면 메서드 호출

### 이벤트처리

```java
//두 클래스가 서로 상속관계이거나 인터페이스를 구현하는 구현체 클래스 이라면
	//반드시 재정의 해야한다.=오버라이드
	@Override
	public void actionPerformed(ActionEvent e) {
		Object obj = e.getSource();
		if(obj==jbtn_account) {
			//사용자가 입력한 정보를 수집해야한다.
			account();
		}
		
		else if(obj==jbtn_exit) {//주소번지가 같니?
			System.exit(0);//자바가상머신과 어플사이의 연결고리를 끊습니다. - thread를 반납, 메모리를 회수한다.=다시 사용할수 없다.
		}
		
		else if(obj==jbtn_create) {//너 생성버튼 누른거니?
			System.out.println("생성버튼 호출성공");//테스트
			
			try {
				inwon = Integer.parseInt(jtf_inwon.getText());
			}catch (NumberFormatException e2) {//가져온 문자가 숫자가 아닌경우
				JOptionPane.showMessageDialog(this, "숫자만 입력하세요.");
				jtf_inwon.setText("");
				return;//actionPerformed를 빠져나간다.			
			}//end of try-catch
			
			jtf_inwon.setEnabled(false);//true:활성화, false:비활성화 //생성버튼을 누르고나면 비활성화된다.
			jbtn_create.setEnabled(false);//버튼 잠그기
			data = new String[inwon][cols.length];//입력된 인원수 x 컬럼명 수 대로 표만들기
			//밑의 세 생성의 얹는 위치와 생성자의 파라미터를 주의하자.
			dtm_list = new DefaultTableModel(data, cols);//생성 cols=cols.length = 컬럼명 = 7개//데이터장착
			jtb_list = new JTable(dtm_list);//생성, 데이터와의 연결//테이블에 데이터 얹기		
			jsp_list = new JScrollPane(jtb_list//테이블 데이터에 스크롤바 얹기
					                    , JScrollPane.VERTICAL_SCROLLBAR_AS_NEEDED
					                    , JScrollPane.HORIZONTAL_SCROLLBAR_AS_NEEDED);
			cont.add("Center", jsp_list);
			cont.revalidate();
			
			this.setLocation(100, 100);//좌표값으로 화면 크기 세팅
			this.pack();//기본크기로 세팅
		}//end of if
	}//end of actionperformed
}//end of class

//인원수는 성적을 구하거나 할때에도 사용해야 하므로 전변으로 한다.
//변수가 유지되어야 한다 : 전변, 변수가 때에따라 계속 변한다 : 지변
//클래스 쪼개기 했을때, 외부에서 사용해야한다. : 전변
```

* Exam1클래스는 actionListener를 impelements하고 있으므로 반드시 actionPerformed를 재정의해야한다.
* 5번 : Object에 이벤트 감지를 꺼내어\(get\) 담는다.
* 6-9번 : "처리"버튼 감지시 account메서드 호출
* 11-13번 : "나가기"버튼 감지시 창 종료
* 15번 : "생성"버튼 감지
* 18번 : 예외처리
* 19번 : inwon은 textField에 입력된 String을 int로 변화 시켜 갖는데 이는 사용자가 int가 아닌 값을 입력할 수 있다. 
* 20-24번 : 해당 오류 처리, 알림창을 띄우고 입력창을 초기화한뒤, 이벤트 진행하지않고 빠져나간다.
* 26-26번  : "생성"버튼이 눌리고나면 입력창과 "생성"버튼을 비활성화한다.
* 28번 : data에 인원수 X 컬럼명수 자료 담기
* 30-35번 : 데이터를 구현할 Table생성하고 붙이기
* 36번 : 갱

