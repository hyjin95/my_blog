# Demo - 입력값 DB에서 가져오기

### DemoBookManager - 메인화면

```java
package design.book;

import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

import javax.swing.JFrame;
import javax.swing.JMenu;
import javax.swing.JMenuBar;
import javax.swing.JMenuItem;

public class DemoBookManager extends JFrame implements ActionListener{
	
	JMenuBar jmb = new JMenuBar();
	JMenu jm_edit = new JMenu("Edit");
	JMenuItem jmi_ins = new JMenuItem("입력");
	JMenuItem jmi_upd = new JMenuItem("수정");
	JMenuItem jmi_det = new JMenuItem("상세보기");
	
	DemoBook db = new DemoBook();
	

	@Override
	public void actionPerformed(ActionEvent e) {
		Object obj = e.getSource();
	
		if(obj==jmi_ins) {
			db.set(e.getActionCommand(),null);
			db.initDisplay();
		}
		else if(obj==jmi_upd) {
			BookVO dVO = new BookVO();
			dVO.setname("혼자 공부하는 자바");
			db.set(e.getActionCommand(),dVO);
			db.initDisplay();
		}
		else if(obj==jmi_det) {
			BookVO dVO = new BookVO();
			dVO.setname("혼자 공부하는 오라클");
			db.set(e.getActionCommand(),dVO);
			db.initDisplay();
		}
	}//end of actionPerformed
	
	public void initDisplay() {
		
		jmi_ins.addActionListener(this);
		jmi_upd.addActionListener(this);
		jmi_det.addActionListener(this);
		
		jm_edit.add(jmi_ins);
		jm_edit.add(jmi_upd);
		jm_edit.add(jmi_det);
		jmb.add(jm_edit);
	
		this.setJMenuBar(jmb);
		this.setSize(500, 400);
		this.setVisible(true);
	}
	
	public static void main (String[] args) {
		 DemoBookManager dbm = new DemoBookManager();
		 dbm.initDisplay();
		
	}//end of main

}//end of calss
```

### DemoBook - 선택 메뉴에 해당하는 메뉴 띄우기, data 가져오기

```java
package design.book;

import javax.swing.JDialog;
import javax.swing.JTextField;

public class DemoBook extends JDialog{
	JTextField jtf_name = new JTextField();
	BookVO bVO= null;
	String title = null;
	
	public DemoBook() { }
	
	public void initDisplay() {
		this.add("North", jtf_name);
		//set("입력",null);//title단위테스트
		this.setTitle(title);//set메서드로 변한 title전변을 갖는다.
		this.setSize(400, 500);
		this.setVisible(true);
	}
	/*****************************************************************
	 * BookManager에서 SELECT한 후 결정된 정보를 set메서드의 파라미터로 넘겨 받는다.
	 * @param title - 사용자가 선택한 버튼이나 혹은 메뉴 내용을 파라미터로 넘겨 받아야한다.
	 * @param bVO - bVO가 null이면 BookManager에서 사용자가 입력버튼을 누른 것이다.
	 * @author 이순신 2020-08-28 수정 : 주석작성자가 누구인지 기록, 작성-수정날짜 기록
	 *****************************************************************/
	public void set(String title, BookVO bVO) {
		//this.setTitle("");//여기다 만들어야 지역변수로 title이 변화한다. 수정->입력->상세보기
		this.title = title;		
		if(bVO!=null) {
			setname(bVO.getname());
		}
	}//end of set method
	
	public void setname(String b_name) {
		jtf_name.setText(b_name);
	}
	
	public String getname() {
		return jtf_name.getText();
	}

	public static void main(String[] args) {
		DemoBook db = new DemoBook();
		db.initDisplay();
	}

}
```

### BookVO - getter & setter

```java
package design.book;
//한번에 한 개 로우만 담는다.
//여러개 로우를 담으려면 Vector나 ArrayList를 써야한다.
public class BookVO {
	
	 private int    b_no     = 0;
	 private String b_name   = null;
	 private String author   = null;
	 private String publish  = null;
	 private String b_info   = null;
	 private String b_img    = null;
	 
	 public int getno() {
		 return b_no;
	 }
	 public void setno(int b_no) {
		 this.b_no = b_no;
	 }
	public String getname() {
		return b_name;
	}
	public void setname(String b_name) {
		this.b_name = b_name;
	}
	public String getAuthor() {
		return author;
	}
	public void setAuthor(String author) {
		this.author = author;
	}
	public String getPublish() {
		return publish;
	}
	public void setPublish(String publish) {
		this.publish = publish;
	}
	public String getinfo() {
		return b_info;
	}
	public void setinfo(String b_info) {
		this.b_info = b_info;
	}
	public String getimg() {
		return b_img;
	}
	public void setimg(String b_img) {
		this.b_img = b_img;
	}

}//end of class
```

