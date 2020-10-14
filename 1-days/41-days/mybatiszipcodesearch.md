# MyBatisZipCodeSearch - Log4j

![Log4j : ZipCode.XML TRACE](../../.gitbook/assets/log2.png)

### log4j.properties

```markup
#log4j.properties
log4j.rootCategory=info, stdout, file
log4j.debug=false
log4j.appender.stdout=com.p6spy.engine.logging.appender.StdoutLogger
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.ImmediateFlush=true
log4j.appender.stdout.Target=System.err

log4j.appender.stdout.layout.ConversionPattern=[%d] [%p] (%13F:%L) %3x - %m%n


log4j.appender.file.DatePattern = '.'yyyy-MM-dd

#emp.xml이나 dept.xml, zipcode.xml의 namespace이름을 등록한다.
log4j.logger.oracle.mybatis.ZipCodeMapper = TRACE

log4j.appender.file.layout=org.apache.log4j.PatternLayout
log4j.appender.file.layout.ConversionPattern=[%d] [%p] (%13F:%L) %3x - %m%n

log4j.logger.java.sql.Connection=INFO
log4j.logger.java.sql.Statement=INFO
log4j.logger.java.sql.PreparedStatement=INFO
log4j.logger.java.sql.ResultSet=INFO
```

## MyBatisZipCodeSearch

### 선언부

```java
public class MyBatisZipCodeSearch extends JFrame implements ItemListener,FocusListener,ActionListener, MouseListener { 
    String zdos3[] = null;
	
	  MyBatisCommonFactory mcf = new MyBatisCommonFactory();
	  SqlSessionFactory ssf = null;
	  SqlSession Session = null;
	
	  List<Map<String,Object>> zdoList = null;
	  int size = 0;
	  Vector<String> v = null;
```

### 생성자

```java
//생성자
	public MyBatisZipCodeSearch() {
		System.out.println("MyBatisZipCodeSearch 호출 성공");
		ssf = mcf.getSqlSessionFactory();
		zdoList = getZdoList();	
	}
```

### 화면처리부

```java
//화면처리부
	public void initDisplay() {

		v = new Vector<>();
		for(int i=0;i<zdoList.size();i++) {
			//Map의 value는 Object타입이므로 {ZDO=XXX}라는 구조체가 Obejct로서 덩어리로 담긴다.
			//이를 그냥 String zdo = rmap.get("ZDO").toString();해서 출력하면 덩어리가 나온다.
			Map<String,Object> rmap = zdoList.get(i);//map으로 key, value를 나눠 담아야한다.
			//rmap.keySet().toArray();//직접 이런 구조를 설계해 보기, 사용자 정의 클래스 활용 연습
			String zdo = rmap.get("ZDO").toString();//mybatis를 통해 값을 꺼낼때, key는 대문자여야한다.
			//mybatis가 map에 put할때 key값을 대문자로 넣기 때문이다.
			v.add(zdo);
		}
		jcb_zdo2 = new JComboBox(v);
		jcb_zdo2.addItemListener(this);
```

### getZdoList 메서드

```java
public List<Map<String,Object>> getZdoList() {
		
		SqlSession sqlSession = mcf.getSqlSessionFactory().openSession();
		List<Map<String,Object>> zdoList = new ArrayList<>();
		
		try {			
			//myBatis에서 자동으로 담아준다.
			zdoList = sqlSession.selectList("getZdoList");
			size = zdoList.size();//초기화
		} catch (Exception e) {
			System.out.println("Exception : "+e.toString());
		} finally {
			if(sqlSession!=null) {
				sqlSession.close();
			}
		}
		return zdoList;
	}//end of getZdoList
```

### refreshData 메서드

```java
//조회메서드
	public void refreshData(String zdo, String dong) {
		System.out.println("zdo : "+zdo+", dong : "+dong);
		List<Map<String,Object>> list = new ArrayList<>();
		Map<String,Object> pMap = new HashMap<>();
		pMap.put("zdo", zdo);
		pMap.put("dong", dong);
		int i = 1;
		SqlSession sqlSession = ssf.openSession();
		try {
			list = sqlSession.selectList("getAddressList", pMap);
			System.out.println("list.size() : "+list.size());
			if(list.size()>0) {
				while(dtm_zipcode.getRowCount() > 0) {
					dtm_zipcode.removeRow(0);
				}
			}
			Iterator<Map<String,Object>> iter = list.iterator();
			Object keys[] = null;

			//조회결과가 한 건도 없는 경우의 화면처리
			if((list==null)||(list.size()<1)) {
				JOptionPane.showMessageDialog(this, "조회결과가 없습니다.");
				return;
			}
			else {
				while(iter.hasNext()) {
					HashMap data = (HashMap)iter.next();
					keys = data.keySet().toArray();
					Vector<Object> oneRow = new Vector<>();
					oneRow.add(0,data.get(keys[0]));//1번 key값은 zipcode
					oneRow.add(1,data.get(keys[1]));//2번 key값은 address
					dtm_zipcode.addRow(oneRow);
				}
			}				
		}catch(Exception e) {
			System.out.println(e.toString());
			e.printStackTrace();
		}finally {
			if(sqlSession!=null) {
				sqlSession.close();
			}
		}//end of try-catch		
	}
```

### main 메서드

```java
//메인메서드
	public static void main(String[] args) {
		MyBatisZipCodeSearch mbzcs = new MyBatisZipCodeSearch();
		mbzcs.initDisplay();
	}//end of main
```

### itemStateChanged

```java
@Override
	public void itemStateChanged(ItemEvent e) {//callback메서드는 시스템이 자동으로 호출한다. 콜백메서드의 파라미터는 객체주입을 받는다.
		Object obj = e.getSource();
		if(obj == jcb_zdo2) {
			if(e.getStateChange() == ItemEvent.SELECTED) {
				System.out.println("111"+zdos3);//zdos3은 지금 null이다.
//				zdos3 = new String[size];//위치
//				v.copyInto(zdos3);
//				zdo = zdos3[jcb_zdo2.getSelectedIndex()];
				zdo = String.valueOf(jcb_zdo2.getSelectedItem());
//				zdo = zdos3[jcb_zdo2.getSelectedIndex()];
				System.out.println(zdo);
			}
		}		
	}//end of if
```

