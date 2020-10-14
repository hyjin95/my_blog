# MyBatisZipCodeSearch 수정

콤보박스에 값이 {ZDO=xx}이렇게 나타나고 있다.  
세션 종료 부분을 추가한다.

```java
public class MyBatisZipCodeSearch extends JFrame implements ItemListener,ActionListener { 
  MyBatisCommonFactory mcf = new MyBatisCommonFactory();
	SqlSessionFactory ssf = null;
	SqlSession Session = null;
	
	String zdos3[] = null;
	
	List<Map<String,Object>> zdoList = null;
	int size = 0;
	Vector<String> v = null;
```

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
public void refreshData(String zdo, String dong) {
		
		SqlSession sqlSession = ssf.openSession();
		try {
							-
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

### ItemListener Override

```java
@Override
	public void itemStateChanged(ItemEvent e) {//callback메서드는 시스템이 자동으로 호출한다. 콜백메서드의 파라미터는 객체주입을 받는다.
		Object obj = e.getSource();
		if(obj == jcb_zdo2) {
			if(e.getStateChange() == ItemEvent.SELECTED) {
				System.out.println("111"+zdos3);//zdos3은 지금 null이다.
				zdos3 = new String[size];//위치
				zdo = String.valueOf(jcb_zdo2.getSelectedItem());
				System.out.println(zdo);
			}
		}		
	}//end of if
```

