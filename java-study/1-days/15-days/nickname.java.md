# NickNameList.java

### 사용된 낱말카드

getter, setter, 생성자, 인스턴스화 분리, 배열, 이벤트처리, UI\(레이아웃\), 추상클래스, 인터페이스, override, 파라미터, 추상메서드, 일반메서드, 변수, 선언과 생성위

### 그려보기

| 대화명 | 성 |
| :---: | :---: |
| xxx | 여 |
| aaa | 남 |
| vvv | 남 |
| ... | ... |

* 필요한 화면구성 - dtm : DefaultTableModel : Header\(대화명, 성별\) 영역 초기화, 읽어오기 및 입력영역 - jtb : JTable : 이벤트 감지시 불러오는 영역 - jsp : JScrollPane
*  Data : 배열, \[1\] \[2\]  - row : 1이지만 대화명이 늘어날수록 row도 증가해야한다. - cols : 목록을 늘리지 않는 한 2로 고정된다. - String\[ \] \[ \]
* 생성자의 역할 - jtb와 jsp를 매칭시켜주는 역할 - ex\) \_ . addActionListener\( \);
* 이벤트 처리 순서 - implements : 구현체 클래스를 결정한다. - JButton 클릭 -&gt; 이벤트 발동 -&gt; 간섭해야함 -&gt; 타이밍필요 -&gt; LifeCycle흐름이해필요 -&gt; 문법에러해결 - 매칭 : \(this\) - 콜백메서드 재정의 : 메서드 이름이 정해진다. - jbtn\_save.addActionListener\(this\); - jbtn\_save : 이벤트 소스 : e.getSource - addAcionListener : 개발자가 매칭해주는 메서드 이름 - \( \) : Object : 클래스 ex\)this

### Data와 Table

| 대화명 | 성별 |
| :---: | :---: |
| test | 남 |
| apple | 여 |

* JTable : Form, 양식
* DefaultTableModel : Data set, 데이터 꾸러미
* 사람이 두명 -&gt; i는 두번 반복, 컬럼 종류도 2개 -&gt;  j도 두번 반복, 배열은 0부터 시작한다. - for\(  ;i&lt;2;  \) { for\(  ;j&lt;2;  \)}}
* 변수\(좌표\) : i, j : 면\(셀\)의 값에 접근하는데 필요하다.
* getValueAt\(i, j\) : 셀의 값을 읽어온다. - get의 리턴타입은 Object이고 get은 값을 꺼내오는 것이므로 파라미터 변수가 두개\(좌표\) 필요하다. - Object는 int, double, boolean 까지 담을 수 있다. - getValue\(i, j\);
* get으로 꺼내온 값을 set으로 저장해서 table 에 올린다. - setValueAt\(obj, i , j\); 

### 성별을 입력하는 창 띄우기

* 성별 : JLabel        : jlb\_gender
* 입력 : JTextField : jtf\_gender : String타입
* Lable과 TestField위치 지정하기 - Layout뭉개기 = null; - setBound\( x , y , width , height\);
* 사용자가 입력창에 입력하는 값 가져오기 - **public void SetGender\(jtf\_gender.getText\( \) \);** - jtf에 쓰기 : jtf\_gender.SetText\( \) - \( \) : jtf에 무엇을 쓸까요? = get에서 반환된 obj값 = jtf\_gender.getText\( \); - getText는 반환타입이 String이다.

## NickNameList.java 

### 선언부

```java
package desigin.test;

import java.awt.event.MouseEvent;
import java.awt.event.MouseListener;

import javax.swing.JFrame;
import javax.swing.JOptionPane;
import javax.swing.JScrollPane;
import javax.swing.JTable;
import javax.swing.table.DefaultTableModel;

//디폴트 생성자는 생략가능 - 파라미터가 없으므로 JVM이 대신 해줄수 있기때문
public class NickNameList extends JFrame implements MouseListener{
	NickNameList nnl = null;
	NickNameListSub nnls = new NickNameListSub(this);
	//table을 바로 보여주느냐 조회하면 보여주느냐, 선언과생성을 같이하느냐 따로하느냐
	static NickNameList nnList = null;//인스턴스화를 하는 새로운 방법-33번
	String cols[]   = {"대화명", "성별"};//선언과 생성 같이
	String data[][] = {
			{"test", "남"}
		   ,{"apple", "여"}
		   ,{"tomato", "여"}
	};//end of data
	DefaultTableModel dtm = new DefaultTableModel(data, cols);
	//테이블 생성시 데이터셋에 대한 헤더 초기화 부분이 필요하다. - dtm이 이때 사용된다.
	JTable jtb = new JTable(dtm);
	JScrollPane jsp = new JScrollPane(jtb
									 ,JScrollPane.VERTICAL_SCROLLBAR_AS_NEEDED
									 ,JScrollPane.HORIZONTAL_SCROLLBAR_AS_NEEDED);
	
```

* NickNameList는 JFrame을 상속하고 MouseListener인터페이스를 시행한다.
* 15번 : Sub클래스와 연결되는 부분이다. Sub창에는 주소번지를 파라미터로하는 생성자가 있다.
* Table의 행 : data\[ \]\[ \]   Table의 열: cols\[ \]
* 24번 : DefaultTableModel클래스의 table생성자 중에 data, cols를 파라미터로 갖는 생성자 호출

### 생성자, 화면구현, main Thread

```java
	public NickNameList() {}
	
	public void initDisplay() {//this는 생략가능
		jtb.addMouseListener(this);
		nnList.add("Center", jsp);
		nnList.setTitle("단톡명단");
		nnList.setSize(450, 500);
		nnList.setVisible(true);
	}

	public static void main(String[] args) {
		nnList = new NickNameList();
		nnList.initDisplay();//한번만 호출		
	}//end of main
```

* 1번 : 디폴트 생성자, 생략가능
* 3번 : 화면구현 메서드
* 4번 : JTable의 addMouseListener생성자를 호출

### MouseListener인터페이스의 추상메서드 override

```java
	@Override
	public void mouseClicked(MouseEvent e) {
		// TODO Auto-generated method stub		
	}

	@Override
	public void mouseEntered(MouseEvent e) {
		// TODO Auto-generated method stub		
	}

	@Override
	public void mouseExited(MouseEvent e) {
		// TODO Auto-generated method stub		
	}

	@Override
	public void mousePressed(MouseEvent e) {
		if(e.getClickCount()==2) {
			int index[] = jtb.getSelectedRows();			
			if(index.length == 0) {//아무것도 선택하지 않았으면
				JOptionPane.showMessageDialog(this, "조회할 데이터를 선택하시오.");			
				return;
			}//end of if
			else if(index.length >= 2) {
				JOptionPane.showMessageDialog(this, "데이터를 한 건만 선택하시오.");
				return;
			}
			else {
				String nickName = (String)dtm.getValueAt(index[0], 0);
				System.out.println("nickName : "+nickName);
				NickNameListSub nnls = new NickNameListSub();
				//이벤트가 일어낫을때 initDisplay호출이므로->true여도되지만 
				//전변에 있을경우에는 반드시 false해주어야한다.
			}			
		}//end of if			
	}
	@Override
	public void mouseReleased(MouseEvent e) {
		// TODO Auto-generated method stub
		}
}//end of class
//Sub호출 -> 인스턴스화 -> 지변인지 전변인지(위치가 달라진다)
//여기서는 28번 else에 해야하는데 그러면 지변이된다.
```

* 사용할 메서드만 재정의한다.
* 18번 : 읽어온 마우스 이벤트 e의 클릭 수가 ==2 라면
* 19번: index배열은 JTable클래스안의 getSelectedRows메서드로 row를 읽어온다,
* 20번 : index\(row\)의 길이가\(선택이\) 0 이라면
* 22번 : 18번 if문 종료
* 24번 : index\(row\)의 길이가\(선택이\)2개 이상이라면
* 26번 : 18번 if문 종료
* 28번 : 나머지의 경우, 1개를 선택했을 경우 - NickNameListSub의 디폴트 생성자 호출 - nnls는 지변으로 다른 함수에서는 사용할수 없다. - 이벤트가 일어났을때 Sub를 호출하기위함이다. 지역변수로 선언되었기때문에 Sub의 initDisplay함수가 true여도 이벤트가 일어나기 전에는 보이지 않지만, 멤버변수로 생성되었을때에는 반드시 false로 해두어야 보이지 않는다. 이때에는 이벤트 발생시 setvisible\(true\)를 적어주어야 한다.

## NickNameListSub.java 

### 선언부

```java
package desigin.test;

import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

import javax.swing.JButton;
import javax.swing.JDialog;
import javax.swing.JLabel;
import javax.swing.JPanel;
import javax.swing.JScrollPane;
import javax.swing.JTextField;

public class NickNameListSub extends JDialog{
	NickNameList nnl     = null;
	JScrollPane  jspLine = null;

	JPanel      jp_center  = new JPanel();
	JLabel      jlb_gender = new JLabel("성별");
	JTextField  jtf_gender = new JTextField();
	
	JPanel      jp_south   = new JPanel();
	JButton     jbtn_save  = new JButton("저장");
	JButton     jbtn_close = new JButton("닫기");
```

* JDialog : 새 창을 띄워주는 클래스
* NickNameListSub클래스는 JDialog를 상속한다.
* 15번 : JScrollPane를 null로 선언한 이유는 jspLine에 jp\_center를 add하면 화면이 덮여저 버려져서 JScrollPane의 생성자를 이용해야 하기 때문에 생성을 나중에 해야한다.
* 17, 21번 : JPanel은 버튼과 textfield를 붙이기 위한 속지이다.

### 생성자

```java
	public NickNameListSub() {
		initDisplay();
	}
	public void update() {
		System.out.println("수정 메서드 호출 성공");
	}
		
	public NickNameListSub(NickNameList nnl) {
		this.nnl = nnl;
	}
```

* 1번 : 디폴트 생성자
* 8번 : NickNameList의 주소번지를 파라미터로 하는 생성자
* 9번 : 멤버변수의 nnl NickNamList 주소번지를 대입한다.

### 화면구현 메서드 : setBounds, ActionListener 다른 사용법

```java
	public void initDisplay() {
	
		jp_center.setLayout(null);
		jlb_gender.setBounds(20, 20, 100, 20);//라벨이 입력창보다 왼쪽(앞)에 위치해야한다.
		jtf_gender.setBounds(70, 20, 100, 20);
		jp_center.add(jlb_gender);
		jp_center.add(jtf_gender);
		jspLine = new JScrollPane(jp_center);
		//위에서 라벨과 입력창의 위치를 결정한 후에, 사용되기 전에 생성되어야 한다.
		
		jbtn_save.addActionListener(new ActionListener() {
			@Override
			public void actionPerformed(ActionEvent ae) {
				update();
			}
		});
		jbtn_close.addActionListener(new ActionListener() {
			@Override
			public void actionPerformed(ActionEvent ae) {
				System.exit(0);
			}
		});
		this.add("South", jp_south);//JPanel속지를 화면의 남쪽에 붙이기
		jp_south.add(jbtn_close);//JPanel에 버튼 붙이기
		this.add("Center", jspLine);//테두리 선
		this.setTitle("상세보기");
		this.setSize(300, 200);
		this.setVisible(true);
		//전변에 인스턴스화를 하면 true면 누르지않아도보인다.
	}//end of initDisplay
```

* JDialog의 디폴트 레이아웃은 BorderLayout\(동,서,남,북\)이다.
* 4번 : Layout 대신 일일이 좌표값을 설정해주어 배치하는 방법 - 직접 좌표값을 활용하므로 레이아웃이 필요없다. setLayout\(null\);은 기본 레이아웃을 뭉개기위한 작업이다.
* 4,5번 : **setBounds\(x,y,whidth,height\)**함수로 좌표를 설정할 수 있다.
* 8번 : jspLine을 jp\_center를 지닌채로 생성한다.
* 11,17번 : **ActionListener인터페이스**를 사용하는 다른 방법 - implements한 경우 : actionListener a = new NickNameList\(\); 
*  implements하지 않고 ActionLitener인터페이스를 사용하는 방법  
  - **addActionListener\(new ActionListener\(actionPerformed\(\) \) \);**

  - add메서드\(new 인터페이스\(추상메서드\(\)구현 \) \);  
  - 사용할 개체에 add메서드를 붙인다.

* 26번 : jspLine을 center에 add한것은, 그 테두리선을 활용하기 위함이다.
* 28번 : true인 이유 : 이 클래스 화면을 호출하는 곳이 NickNameList의 이벤트가 일어난 뒤이기 때문 

### 값 담아오기 : getter, setter함수

```java
	////////////////////[setter and getter[///////////////////
	public void getGender(String gender) {
		jtf_gender.setText(gender);
	}	
	public void getGender() {
		jtf_gender.getText();//가져온값 member변수에 담기
		MemberVO mVO = new MemberVO(null, jtf_gender.getText());//생성자의 파라미터를 사용한 초기화
		//MemberVO mVO = new MemberVO();-메서드를 이용한 초기화
		//mVO.setGender(jtf_gender.getText());-메서드를 이용한 초기화2
	}	
	////////////////////[setter and getter[///////////////////	
}//end of class
```

* getter함수 : 읽어오기. 리턴타입이 필수
* setter함수 : 쓰기, 저장하기. 파라미터와 리턴타입 모두 필수
* 2,5번 : get함수이지만 리턴타입이 void인 이유는 2번은 파라미터로 String타입을 갖고, 5번은 가져오는 값이 String이기 때문
* 2번 : String타입 파라미터를 갖는 읽어오는 메서드
* 3번 : jtf\_gender에 입력된 text를 파라미터인 gender지역변수에 저장하는 구현문 
* 5번 : 파라미터가 없는 get함수
* 6번 : jtf\_gender멤버 변수에 읽어온값을 담는다.

## MemberVO.java 

### 데이터를 private멤버변수로 대입해주는 클래스

```java
package desigin.test;

public class MemberVO {
	private String nickName = null;
	private String gender   = null;
	
	public MemberVO() {}
	
	public MemberVO(String nickName, String gender) {//갯수만큼 다 써주어야함
		this.nickName = nickName;
		this.gender = gender;
	}
	
	public String getNickName() {
		return nickName;
	}
	public void setNickName(String nickName) {
		this.nickName = nickName;
	}
	public String getGender() {
		return gender;
	}
	public void setGender(String gender) {
		this.gender = gender;
	}
}
```

* 4,5번 : nickName, gender변수는 담는 값이 변하기 때문에 null로두고, 타입이 String이여야하기 때문에 private멤버변수로 선언한다.
* 7번: 디폴트생성자, 생략가능
* 9번 : 데이터를 파라미터로 갖는 생성자 - 데이터 컬럼명을 모두 파라미터에 넣어주어야한다.
* 10,11번 : 입력된 파라미터들을 멤버변수에 대입, 초기화한다. \(소스\) - 10,11번 드래그 후 우클릭으로 get, set함수를 만들 수 있다.
* 14번 : String타입의 get함수는 nickName을 반환한다.  
* 17번 : String타입 파라미터를 갖는 set함수이다.
* 18번 : 멤버변수 nickName에 입력된 nickName을 저장, 초기화한다.

