# ZipcodeSearch - JcomboBox

### import

```java
public class ZipCodeSearch extends JFrame implements FocusListener, ActionListener {
import java.awt.event.FocusListener;
```

### 선언부

```java
	String zdos[] = {"전체","서울", "경기", "강원"};//String 타입 배열생성
	String zdos2[] = {"전체","부산", "충남", "인천"};//String 타입 배열생성
	Vector<String> vzdos = new Vector<>(); 
	JComboBox jcb_zdo = new JComboBox(zdos);
	JComboBox jcb_zdo2 = null;
```

### 화면구현부

```java
public void initDisplay() {
    jtb_zipcode.requestFocus();
		jtf_search.addFocusListener(this);
		
		jp_north.setLayout(new BorderLayout());
		jp_north.add("West", jcb_zdo);
		
		vzdos.copyInto(zdos2);
		jcb_zdo2 = new JComboBox(vzdos);
}
```

