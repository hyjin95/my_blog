# ZipcodeSearch - JcomboBox, FocusListener



## ZipCodeSearch.java

### import

```java
import java.awt.event.FocusListener;
```

### 선언부

```java
public class ZipCodeSearch extends JFrame implements FocusListener, ActionListener {
  //1번 생성자
	String zdos[] = {"전체","서울", "경기", "강원"};//String 타입 배열생성
	JComboBox jcb_zdo = new JComboBox(zdos);
	
	//2번생성자
	String zdos2[] = {"전체","부산", "전남", "대구"};//String 타입 배열생성
	Vector<String> vzdos = new Vector<>();//현재 vzdos.size()는 0
	JComboBox jcb_zdo2 = null;//선언과 생성을 분리해야한다.
	/* String배열에 있는 정보들을 굳이 벡터에 담아야한다.
	 * 생성자 선택을 Vector타입으로 결정했기 때문이다.
	 * 벡터 자체는 값은 가지고 있지 않은 상태이다.
	 * 값은 1차 배열안에 초기화되어있다.
	 * 이것을 벡터에 재 초기화해야한다.
	 * 그 후에 콤보박스를 생성하고 화면에 장착해야 값이 담긴다.
	 */
```

### 화면구현부

```java
public void initDisplay() {
	  //2번 생성자
		for(int i=0;i<zdos.length;i++) {
			vzdos.add(zdos2[i]);
		}
		jcb_zdo2 = new JComboBox(vzdos);
		vzdos.copyInto(zdos);
		//2번 생성자 끝
		
    jtb_zipcode.requestFocus();
		jtf_search.addFocusListener(this);
		
		jp_north.setLayout(new BorderLayout());
		jp_north.add("West", jcb_zdo2);		
}
```

### SELECT문과 DB연결 메서드

```java

```

### Override

```java

```

