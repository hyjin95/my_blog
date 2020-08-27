# ZipCodeSearch.java - 우편번호 조회

## MemberShip클래스에서 조회한 우편번호를 읽고 쓰기위해 추가된 부분

### 선언부

```java
public class ZipCodeSearch extends JFrame implements ItemListener,FocusListener,ActionListener, MouseListener {
    MemberShip memberShip = null;
```

### 인스턴스 생성자

```java
public ZipCodeSearch(MemberShip memberShip) {
		this();//화면이 열릴때 디폰트생성자를 호출해야한다. 디폴트생성자를 호출하는법.
		this.memberShip = memberShip;		
	}
```

### 화면구현

```java
public void initDisplay() {
    jtb_zipcode.addMouseListener(this)
```

### 메인 메서드

```java
public static void main(String[] args) {
		ZipCodeSearch zcs = new ZipCodeSearch();
		zcs.initDisplay();
	}//end of main
```

### Override

```java
	@Override
	public void mousePressed(MouseEvent de) {
		if(de.getClickCount()==2) {
			int index[] = jtb_zipcode.getSelectedRows();			
			if(index.length == 0) {//아무것도 선택하지 않았으면
				JOptionPane.showMessageDialog(this, "조회할 데이터를 선택하시오.");			
				return;
			}//end of if
			else if(index.length >= 2) {
				JOptionPane.showMessageDialog(this, "데이터를 한 건만 선택하시오.");
				return;
			}
			else {//하나 선택시 값을 담는다.
				int zipcode = (Integer)dtm_zipcode.getValueAt(index[0], 0);
				String address = (String)dtm_zipcode.getValueAt(index[0], 1);
				System.out.println("우편번호 : "+zipcode);
				System.out.println("주소 : "+address);
				memberShip.jtf_zdo.setText(String.valueOf(zipcode));
				memberShip.jtf_zdo2.setText(String.valueOf(address));
			}			
		}//end of if			
	}
```

