---
description: 2020.08.14 - 13일차
---

# 13 Days - 클래스 쪼개기

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261 : Oracle.com
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5

### 클래스를 쪼갤때 고려해야 할 것

1. 변수의 갯수와 위치
2. 메서드 - 파라미터\(요청\), 리턴\(응답\)
3. 생성자 - 파라미터O,  리턴타입X - 역할 : 클래스와 클래스를 매칭시켜준다.
4. 클래스의 상속관계

### 예제

Sub.java에서 Main.java의 등록버튼의 주소번지를 사용하는 두 가지 방법

### 1. 메서드를 이용한다

```java
package bookapp;

import javax.swing.JButton;
import javax.swing.JFrame;

public class Main extends JFrame {
	JButton jbtn_add = new JButton("등록");
	Main m = null;
	
	public Main getInstance() {
	if(m!=null) { // null 체크를 하여 복사본이 만들어지는 것을 방지
		m = new Main();// 하나 - 싱글톤
	}
	return m;
	}
	
	public JButton getButton() {
		return jbtn_add;
	}
	public static void main(String[] args) {
		Main m = new Main();
		Sub sub = new Sub(m);
	}
}
```

* 객체를 메서드라는 수단을 사용하여 주입 받겠다.
* 11-15번 : 인스턴스화 주소번지를 하나로만 지정하는것을 싱글톤이라고 한다. 한정된 자원을 차례대로, 공평하게 여러 클래스가 사용하는 것이다.

### 2. 생성자를 이용한다

```java
package bookapp;

import java.util.Vector;

import javax.swing.JDialog;

public class Sub extends JDialog {
	Main m = null;
	
	public Sub(Main m) {
		this.m = m;
		m.getButton().setText("가입");
		this.add("North", m.getButton());
		this.setTitle("Sub화면입니다.");
		this.setSize(300, 200);
		this.setVisible(true);				
	}
```

* JDialog : 새 창을 띄우는 클래스
* 12번 : setter, getter로 객체를 생성할 수 있다.

## BookManager.java 쪼개기

버튼을 누르는 Event 발생시 새 창을 띄우게한다.

### 생각하기

1. 입력을 누르면 새 '입력'창이, 수정을 누르면 새 '수정'창이, 보기를 누르면 새 '보기'창을 띄워야한다.
2. 사용할 메서드 - public ActionPerformed\(ActionEvent\):void - public bookIns\(n개\):int : 건수를 입력하는 메서드이므로 int타입을 갖는다. \(책제목 등을 입력\) - public bookUpd\(n개\):int : 위와 같다. \(이름, 번호, 주소 등을 수정\) - public booksel\(\_.no\):BookVO : 책넘버를 검색하면 가격, 목차 등 여러 정보를 보여줘야한다. - public exit\( \); : 종료기능, 파라미터와 리턴타입은 없다.
3. BookManager -&gt; EventHandler -&gt; BookCURD - 화면에서 입력이 일어나면 Event가 새 창을 호출한다. - 기본적으로는 이벤트안에 CURD, Manager안에 이벤트가 인스턴스화 되어야 하지만, 유지보스 측면에서 봤을때 인스턴스화가 한 클래스 안에 있는게 좋다.
4. JFrame이 상위, JDIalog클래스가 하위 개념으로 메인이 꺼지면 생성된 서브 창도 꺼진다.

### BookManager.java

```java
package bookapp;

import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

import javax.swing.JFrame;
import javax.swing.JMenu;
import javax.swing.JMenuBar;
import javax.swing.JMenuItem;
import javax.swing.JSeparator;

//public class BookManager extends JFrame implements ActionListener {
public class BookManager extends JFrame {
	JMenuBar  jbm	   = new JMenuBar();
	JMenu     jm_edit  = new JMenu("Edit");
	JMenuItem jmi_ins  = new JMenuItem("입력");
	JMenuItem jmi_upd  = new JMenuItem("수정");
	JMenuItem jmi_det  = new JMenuItem("상세보기");
	
	JSeparator js1	   = new JSeparator();
	JMenuItem jmi_exit = new JMenuItem("나가기");
	BookManagerEventHandler beh = new BookManagerEventHandler(this);
	BookCRUD BCRUD = new BookCRUD();
	
	public BookManager() {
		
	}
	public void initDisplay() {
		
		//이벤트 소스와 이벤트 처리 핸들러 클래스를 매칭하기
		jmi_ins.addActionListener(beh);//this는 BookManager 이클래스 안에 actionPerformed가 있어야한다.
		jmi_upd.addActionListener(beh);
		jmi_det.addActionListener(beh);
		jmi_exit.addActionListener(beh);
		jm_edit.add(jmi_ins);
		jm_edit.add(jmi_upd);
		jm_edit.add(jmi_det);
		jm_edit.add(js1);
		jm_edit.add(jmi_exit);
		jbm.add(jm_edit);
		this.setJMenuBar(jbm);
		
		this.setTitle("도서관리 시스템 Ver1.0");
		this.setSize(700,450);
		this.setVisible(true);
	}
	public static void main(String[] args) {
		JFrame.setDefaultLookAndFeelDecorated(true);
		BookManager bm = new BookManager();
		bm.initDisplay();
	}	
}
```

* UI와 이벤트 감지를 담당하는 클래스
* 22번 : BookManagerEventHandler클래스를 인스턴스화 하는데, Handler클래스에서 주소번지를 파라미터로 갖는 생성자를 호출한다.
* 25번 : 이 클래스의 디폴트 생성자
* 31-34번 : 이벤트의 감지는 이 클래스에서 일어나지만 이벤트 처리기능은 다른 클래스에 있다.

### BookManagerEventHandler.java

```java
package bookapp;

import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

public class BookManagerEventHandler implements ActionListener {
	
	BookManager bm = null;//주의사항 : 절대로 new하면 안됨 - 왜냐하면 복제본이 만들어지는 것이므로.
//	//BookManager bm = new BookManager(); 	

	public BookManagerEventHandler(BookManager bm) {
		this.bm = bm;
	}

	@Override
	public void actionPerformed(ActionEvent e) {
		String label = e.getActionCommand();
		System.out.println("label===>" +label);
		if("입력".equals(label)){//???????????????????????????????????
		    System.out.println("입력을 눌렀는가?");
		    bm.BCRUD.setTitle(label);
		    bm.BCRUD.setSize(500, 400);
		    bm.BCRUD.setVisible(true);
		}
		
		else if("수정".equals(label)) {
			 System.out.println("수정버튼 눌렀음");
			 bm.BCRUD.setTitle(label);
			 bm.BCRUD.setSize(500, 400);
			 bm.BCRUD.setVisible(true);
		}
		else if("상세보기".equals(label)) {
			 System.out.println("상세보기버튼 눌렀음");	
			 bm.BCRUD.setTitle(label);
			 bm.BCRUD.setSize(500, 400);
			 bm.BCRUD.setVisible(true);
		}
		else if("나가기".equals(label)) {
			 System.out.println("나가기버튼 눌렀음");
			 System.exit(0);
		}
	}
}
```

* 이벤트 담당 클래스
* 8번 : null해야 원본값을 그대로 가져올 수 있다.
* 11-13번 : 주소번지를 담는 생성자이다. 
* 17번 : 읽어온\(get\) 것을 String타입의 label에 담는다.
* 21-23번 : BookManager에 인스턴스화 되어있는 CURD를 호출한다.

### BookCURD.java

```java
package bookapp;

import javax.swing.JDialog;

public class BookCRUD extends JDialog {
	public void initDisplay() {
		this.setVisible(false);
	}
	/*
	 * public static void main(String args[]) { new BookCRUD().initDisplay(); }
	 */
}
```

* 새 창\(JDialong\) 띄우기 클래스
* 이 창은 평소에는 보이지 않다가 event 발생시 보여야 하므로 setVisible의 기본값은 false이다.

### 숙제

8일차에 만들었던 야구 숫자 게임을 4명의 팀원이서 클래스를 나누어 구현시켜보자.  
main, ui, event, logic

후기 : 첫 팀 숙제를 주셨다. 우리가 잘 연결 할 수 있을까..? 화이팅화이팅!

