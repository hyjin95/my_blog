# MyBatisZipCodeSearch

## MyBatisCommonFactory.java

```java
package com.util;

import java.io.FileNotFoundException;
import java.io.IOException;
import java.io.Reader;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
//xml과 자바가 만나는 부분 설계 클래스 --공통팀
public class MyBatisCommonFactory {
	//선언부
	//xml문서로부터 객체를 주입받아야 하므로 단독으로 인스턴스화해버리면 안된다.
	public SqlSessionFactory sqlSessionFactory = null;
	public SqlSession		 session = null;//오라클에 DML을 요청하는 클래스
	//위 두 클래스는 서로 의존관계에 있다. 연결통로 생성 -> 요청클래스 생성이 이루어져야한다.
		
	//초기화
	public void init() {
		try {
			//MapperConfig.xml문서에서 오라클 서버에 대한 정보를 스캔해야한다.--1
			//물리적으로 떨어져있는 오라클과의 연결통로를 생성해야한다. = SqlSessionFactory(myBatis제공 클래스) --2
			String resource ="oracle/mybatis/MapperConfig.xml";			
			Reader reader = Resources.getResourceAsReader(resource);
			//builder클래스로 sqlSessionFactory를 인스턴스화
			sqlSessionFactory = new SqlSessionFactoryBuilder().build(reader);//Factory클래스가 먼저 생성되어야한다.		
			System.out.println("SqlSessionFactory(SqlSessionFactoryBean) "+sqlSessionFactory);//객체주입이 되었는지 확인하는 단위테스트, null이 나오면 안된다.
		} catch (FileNotFoundException fe) {
			System.out.println("FileNotFoundException : "+fe.getMessage());
		} catch (IOException ie) {
			System.out.println("IOException : "+ie.getMessage());
		}
	}
	//싱글톤 패턴으로 개발을 전개해야 할 때는 메서드로 객체주입 받도록 한다.
	public SqlSessionFactory getSqlSessionFactory() {
		init();//sqlSessionFactory의 객체 생성이 일어난다.
		return sqlSessionFactory;//init메서드를 거치지 않으면 null
	}
}
```

## ZipCode.XML

```markup
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="oracle.mybatis.ZipCodeMapper">
 <select id="getZdoList" parameterType="map" resultType="Map">
		SELECT '전체' zdo FROM dual
		UNION ALL
		SELECT zdo
		  FROM (SELECT distinct(zdo) zdo
		        FROM zipcode_t
		        ORDER BY zdo asc)
 </select>
 
 <select id="getAddressList" parameterType="map" resultType="Map">
 		SELECT zipcode, address FROM zipcode_t
	<where>
	 <if test="zdo!=null and zdo.length()>0">
	 	AND zdo=#{zdo}
 	 </if>
	 <if test="dong!=null">
	 	AND dong like '%'||#{dong}||'%'
 	 </if>
 	</where>
 </select>
</mapper>
```

## MyBatisZipCodeSearch

### 선언부

```java
public class MyBatisZipCodeSearch extends JFrame{

  MyBatisCommonFactory mcf = new MyBatisCommonFactory();
	SqlSessionFactory ssf = null;
	SqlSession Session = null;
	
	List<Map<String,Object>> zdoList = null;
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

### getZdoList 메서드 

```java
public List<Map<String,Object>> getZdoList() {
		
		SqlSession sqlSession = mcf.getSqlSessionFactory().openSession();
		List<Map<String,Object>> zdoList = new ArrayList<>();
		
		try {			
			//myBatis에서 자동으로 담아준다.
			zdoList = sqlSession.selectList("getZdoList");
		} catch (Exception e) {
			System.out.println("Exception : "+e.toString());
		}		
		return zdoList;
	}//end of getZdoList
```

### initDispaly 메서드

```java
public void initDisplay() {
		Vector<String> v = new Vector<>();
		for(int i=0;i<zdoList.size();i++) {
			String zdo = rmap.get("ZDO").toString();
			v.add(zdo);
		}
		jcb_zdo2 = new JComboBox(v);
}
```

### refreshData 메서드 

```java
//조회메서드
	public void refreshData(String user, String dong) {
		System.out.println("zdo : "+zdo+", dong : "+dong);
		List<Map<String,Object>> list = new ArrayList<>();
		Map<String,Object> pMap = new HashMap<>();
		pMap.put("zdo", zdo);
		pMap.put("dong", dong);
		int i = 1;
		SqlSession sqlSession = ssf.openSession();
		try {
			list = sqlSession.selectList("getAddressList", pMap);
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
		}//end of try-catch		
	}
```

## 기존 ZipCodeSearch.java - refreshDate\( \)

{% page-ref page="zipcodesearch.md" %}

